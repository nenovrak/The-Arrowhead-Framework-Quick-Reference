The procedure of signing Arrowhead certificates at different levels is easy and you can use a tool like [KeyStore explorer](https://keystore-explorer.org/downloads.html) to do it. In order to create a local cloud certificate, perform the following in Keystore explorer:

1. Open the Master.p12 certificate with KeyStore explorer, 
2. Right-click on the certificate and then choose "sign new key pair" (input the master keystore password again). Select RSA with Key Size: 2,048. 
3. Choose Version 3, SHA-256 with RSA, set a validity period (e.g. 10 years), then click on the little address book marked with "@" in the lower right corner.
4. Input your Common Name as follows: local_cloud_name.organisation.arrowhead.eu (e.g. secureTempCloud.ltu.arrowhead.eu)
5. Input the other information about your certificate
6. Now generate the new key pair. Accept the New Key Pair Alias as  suggested by KeyStore explorer (e.g. secureTempCloud.ltu.arrowhead.eu (arrowhead.eu)
7. Choose a password for your new key pair.
8. Now drag-drop your newly created certificate to a new tab in KeyStore explorer (drag the certificate to the area right next to an open tab). Then select this tab and choose "Save as" and save the certificate as a .p12 file.
9. (NOTE This step can probably be replaced with closing the master.p12 tab without saving). Now go back to the master.p12 keystore (switch tab). This keystore should not have your local cloud certificate in it since its only purpose is to be the root certificate for the Arrowhead framework. Therefore you must now remove your certificate from this. Do this by navigating back to this tab and right-click on you certificate and choose "delete". Then choose save to put the master.p12 in its original form.

In order to create a certificate for a System in your Local Cloud, you repeat the exact same process as above but using the Local Cloud certificate you just created to sign the System certificate. (I.e. repeat the steps above but use your Local Cloud certificate instead of the Master.p12 (Arrowhead root) to sign new certificates).

Finally, you need to update the configuration files of all Local Cloud Systems to have their trust-stores point to the Local Cloud certificate, and their keystores to point to their own System certificate.