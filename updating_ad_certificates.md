# Updating the Turku AD certificates

Until the internal-gw in eVaka implements fetching the AD metadata
automatically, the AD certificates have to be kept up to date manually.

## Symptoms of the certificate having been changed in the AD

If the certificate has been changed in the AD without having been
correspondingly updated in eVaka, the following will be observed:

 - The AD logon will fail with the message "Kirjautuminen epäonnistui", but
   the other logon methods (ie. citizen, service provider etc.) will work
 - The evakaturku-internal-gw log group will contain messages saying "Failed to
   authenticate user. Description: Could not parse SAML message. Details:
   Error: SAML provider returned Responder error: RequestDenied". Note that
   these will not be found by searching for the string "ERROR"

## AD certificates in the eVaka repository

The AD certificates are stored in the eVaka core repository, under path
apigw/config/certificates . There are three files named
turkuad-internal-{prod,migration,staging}.pem. Currently all three files
contain the same certificate, therefore they should all be updated with the
same new certificate at the same time. The certificate file which the
environments use is configured in the infra repository as the variable
ad_saml_public_cert.

Note that since the certificate to be used in configured in the infra, it
may be that the certificate can't be proactively changed in eVaka. The
following paragraph is written presuming that it could.

If the AD certificate has already been changed, you can overwrite the existing
files. If you are preparing for a forthcoming change, you can add new
certificate files in the core repository and add the names of the new files
in apigw/src/shared/certificates.ts . That file contains the names array
which contains the names of the certificate files.

## Obtaining the new certificates

If the AD certificate has already been changed, you can manually obtain the
AD metadata XML file from address
https://sts.edu.turku.fi/federationmetadata/2007-06/federationmetadata.xml .
Inside the root EntityDescriptor element the initial element should be
a ds:Signature element, which eventually contains an X509Certificate element
containing the certificate you should use in the eVaka certificate files.

Ideally Turku should inform you about the certificate change beforehand, in
which case you will have to obtain the new certificate from the Turku
administrators in some other way.
