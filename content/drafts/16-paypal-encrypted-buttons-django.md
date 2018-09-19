---
title: Paypal encrypted buttons with Django
slug: paypal-encrypted-buttons-django
date: 2008-10-09 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

<strong>Update (13/03/09):</strong> If you're reading this, you should probably also read <a href="http://jonatkinson.co.uk/more-paypaldjango/">this</a>.

I'm currently writing an invoicing application for <a href="http://www.mampi.co.uk">Mampi</a>, and we decided to use <a href="http://www.paypal.co.uk">Paypal</a> as our payment processor. While I know plenty of people think that Paypal are evil, for processing small volume transactions via UK debit and credit cards, they're far cheaper (considering both integration costs and transaction fees) than any of the alternatives.

The standard way to place a 'buy now' button on a website to to create the HTML for the button on the Paypal website, then paste it into your site's HTML. Something like this:

<pre>&lt;form target=&quot;paypal&quot; action=&quot;https://www.paypal.com/cgi-bin/webscr&quot;  method=&quot;post&quot;&gt;
    &lt;input type=&quot;hidden&quot; name=&quot;business&quot; value=&quot;sales@somecompany.com&quot;&gt;
    &lt;input type=&quot;hidden&quot; name=&quot;cmd&quot; value=&quot;_xclick&quot;&gt;
    &lt;input type=&quot;hidden&quot; name=&quot;item_name&quot; value=&quot;Widget&quot;&gt;
    &lt;input type=&quot;hidden&quot; name=&quot;item_number&quot; value=&quot;12345&quot;&gt;
    &lt;input type=&quot;hidden&quot; name=&quot;amount&quot; value=&quot;20.00&quot;&gt;
    &lt;input type=&quot;hidden&quot; name=&quot;currency_code&quot; value=&quot;GBP&quot;&gt;
    &lt;input type=&quot;image&quot; name=&quot;submit&quot;border=&quot;0&quot; src=&quot;https://www.paypal.com/en_US/i/btn/btn_buynow_LG.gif&quot; alt=&quot;PayPal - The safer, easier way to pay online&quot;&gt;
&lt;/form&gt;</pre>

I hate this. I realise that Paypal have some systems in place to prevent tampering with this form, but having the price and name of the item in the plain HTML still makes me very uncomfortable. Fortunately, Paypal offer the ability to encrypt this information. It's relatively straight-forward, you simple convert the above fields to text, separated by newline characters:

<pre>business=sales@somecompany.com
cmd=_xclick
item_name=Widget
item_number=12345
amount=20.00
currency_code=GBP</pre>

.then encrypt this data with your OpenSSL key. The resulting block will look something like this:

<pre>-----BEGIN PKCS7-----
MIIHmwYJKoZIhvcNAQcDoIIHjDCCB4gCAQAxggEwMIIBLAIBADCBlDCBjjELMAkG
A1UEBhMCVVMxCzAJBgNVBAgTAkNBMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQw
- lots more cyphertext here -
lyypJ1Kf7oVJ4CmuC0MJLOyxK5fzlBbpjB9TxVFNyQ0e9UQF5bV80f29LIvNR2rV
8o0IVfkFRHWuf7Jo1w/SEvYtuU7HnwYdldD2wVeAVQ==
-----END PKCS7-----
</pre>

So, how to achieve this with Python? The application I'm writing uses Django, and the only official examples given by Paypal were written in C# and Java, which isn't much use to me. Fortunately, I found <a href="http://blog.mauveweb.co.uk/category/python/">this blog post</a>, by Daniel Pope, which helped clarify a little, but I hope this serves as a most complete example.

First, you need to create and sign your OpenSSL certificate, and upload it to Paypal (then download their certificate). This process is fairly well documented in the <a href="https://www.paypal.com/en_US/pdf/PP_WebsitePaymentsStandard_IntegrationGuide.pdf">Paypal Integration Guide</a>, but I'll reproduce the steps here for clarity. To create your private key:

<pre>$ openssl genrsa -out my-prvkey.pem 1024</pre>

Then create your public key:

<pre>$ openssl req -new -key my-prvkey.pem -x509 -days 365 -out my-pubcert.pem</pre>

You now need to go and upload your public key to the Paypal website. Login to your account, then click Profile, Seller Preferences, Encrypted Payment Settings. On this page is an upload form for your public key. Once you've uploaded this, you'll see your certificate ID, which you'll need to note. Download the Paypal public certificate from the same page. 

Now, place all three files, the public key, the private key, and the Paypal certificate into a folder called 'certs' under your Django project root. Edit the settings.py file accordingly:

<pre>MY_KEYPAIR = '/path/to/certs/my-prvkey.pem'
MY_CERT = '/path/to/certs/my-pubcert.pem'
PAYPAL_CERT = '/path/to/certs/paypal_cert.pem'
MY_CERT_ID = 'ABCDEF12345'</pre>

Now, create a paypal.py file in the appropriate application (in my case, it was /invoices/paypal.py). This file will contain the function to create the encrypted block. Here it is:

<pre>from M2Crypto import BIO, SMIME, X509
from django.conf import settings
	
def paypal_encrypt(attributes):
	
	plaintext = ''
	
	for key, value in attributes.items():
		plaintext += u'%s=%s\n' % (key, value)
	
	plaintext = plaintext.encode('utf-8')
	
	# Instantiate an SMIME object.
	s = SMIME.SMIME()
	
	# Load signer's key and cert. Sign the buffer.
	s.load_key_bio(BIO.openfile(settings.MY_KEYPAIR), BIO.openfile(settings.MY_CERT))
	
	p7 = s.sign(BIO.MemoryBuffer(plaintext), flags=SMIME.PKCS7_BINARY)
	
	# Load target cert to encrypt the signed message to.
	x509 = X509.load_cert_bio(BIO.openfile(settings.PAYPAL_CERT))
	sk = X509.X509_Stack()
	sk.push(x509)
	s.set_x509_stack(sk)
	
	# Set cipher: 3-key triple-DES in CBC mode.
	s.set_cipher(SMIME.Cipher('des_ede3_cbc'))
	
	# Create a temporary buffer.
	tmp = BIO.MemoryBuffer()
	
	# Write the signed message into the temporary buffer.
	p7.write_der(tmp)
	
	# Encrypt the temporary buffer.
	p7 = s.encrypt(tmp, flags=SMIME.PKCS7_BINARY)
	
	# Output p7 in mail-friendly format.
	out = BIO.MemoryBuffer()
	p7.write(out)
	
	return out.read()</pre>

This code is all fairly straightforward (you'll need to M2Crypto Python module, which is an apt-get/yum away on most servers, though if you're building on OSX, you might want to read <a href="http://jonatkinson.co.uk/installing-m2crypto-osx/">my previous instructions</a>), it simply takes a dictionary of attributes, creates the plaintext in a UTF-8 string, then encrypts that string using the keys you generated earlier. Here is a quick demonstration of how to use it in a Django view:

<pre>def pay(request, invoice_id):
	"""This view displays an encrypted PayPal 'buy now' button"""
	
	invoice = get_object_or_404(Invoice, id = 1)
	
	attributes = {}
	attributes['cmd'] = '_xclick' 
	attributes['business'] = 'sales@yourcompany.com'
	attributes['item_name'] = invoice.item_name
	attributes['amount'] = invoice.amount
	attributes['currency_code'] = 'GBP'
	
	encrypted_block = paypal_encrypt(attributes)
	
	return render_to_response('pay.html', {'encrypted_block': encrypted_block})
</pre>

This demonstrates how to create the attribute dictionary. Notice how the keys mirror the field names on the plaintext form I showed above? The Paypal documentation describes all the valid attributes which you can pass to Paypal, shown here is a fairly minimal set, but it is enough to process a payment.

To complete the functionallity, here is the view code which will insert the encrypted block into a valid Paypal form:

<pre>&lt;form target=&quot;paypal&quot; action=&quot;https://www.paypal.com/cgi-bin/webscr&quot; method=&quot;post&quot;&gt;
	&lt;input type=&quot;hidden&quot; name=&quot;cmd&quot; value=&quot;_s-xclick&quot; /&gt;
	&lt;input type=&quot;hidden&quot; name=&quot;encrypted&quot; value=&quot;{{ encrypted_block }}&quot; /&gt;
	&lt;input class=&quot;button pay&quot; type=&quot;submit&quot; name=&quot;submit&quot; value=&quot;Pay Invoice&quot; /&gt;
&lt;/form&gt;</pre>
            
            