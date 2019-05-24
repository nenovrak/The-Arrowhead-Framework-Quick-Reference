The procedure of signing Arrowhead certificates at different levels is easy and you can use a tool like [KeyStore explorer](https://keystore-explorer.org/downloads.html) to do it. In Keystore explorer this is achieved in the following way: Open the Master.p12 certificate with KeyStore explorer, 
2. Right-click on the certificate and then choose "sign new key pair" (input the master keystore password again). Select RSA with Key Size: 2,048. 
3. Choose Version 3, SHA-256 with RSA, set a proper validity period (e.g. 10 years), then click on the little address book marked with "@" in the lower right corner.
4. Input your Common Name as follows: local_cloud_name.organisation.arrowhead.eu (e.g. secureTempCloud.ltu.arrowhead.eu)
5. Input the other information about your certificate
6. Now you go ahead and generate the new key pair. Accept the New Key Pair Alias suggested by KeyStore explorer (e.g. secureTempCloud.ltu.arrowhead.eu (arrowhead.eu)
7. Choose a password for your new key pair.
8. Now drag-drop your newly created certificate to a new tab. Then go to this tab and choose "Save as" and save it as a .p12 file.
9. Now go back to the master.p12 keystore. This keystore should not have your local cloud certificate in it since its only purpose is to be the root certificate for the Arrowhead framework. Therefore you must now remove your certificate from this. Do this by navigating back to this tab and right-click on you certificate and choose "delete". Then choose save to put the master.p12 in its original form.

In order to create a certificate for a System in the Local Cloud you repeat the exact same process as above but using the Local Cloud certificate you just created to sign the System certificate. (Repeat the steps but replace the Master.p12 with your Local Cloud certificate).

Note that you need to update the configuration files of all systems to have their trust-stores point to the Local Cloud certificate.
