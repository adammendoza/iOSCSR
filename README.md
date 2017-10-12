# iOSCSR
Generate CSR (Certificate Signing Request) with RSA or EC

To use in code

Download CryptoAPI folder, Extract and pick CryptoAPI.framework and drag into your code, add this into Ebedded Binaries. 
import #import <CryptoAPI/CryptoAPI.h> into your class where you want to generate CSR and write following code.

      NSString * currentlLoginEmailAddress = @"test102@test.com"; // Unique string use as alias for Keypairs
      NSString * bundleID = @"com.myCompany.myApp"; // Bundle ID of your App use as Application Tag
      NSString * userName = @"user name"; // User Name or name for organization which use in Certificate
      NSString * randomNumber = @"453963013"; // Any Serial number which use in Certificate.

      KeyPairAttributes * keyPairAttributes = [[KeyPairAttributes alloc] init];
      keyPairAttributes.keyLabel = currentlLoginEmailAddress;
      keyPairAttributes.keySize = KeyPairAttributes.keySize256;
      keyPairAttributes.keyStorageType = KeyPairAttributes.Hardware;
      keyPairAttributes.keyType = KeyPairAttributes.EC;
      keyPairAttributes.keyApplicationTag = bundleID;

      KeyPairManager * keypairManager = [[KeyPairManager alloc] init];
      [keypairManager deleteKeyPairWithKeyTag:currentlLoginEmailAddress];
      [keypairManager generateKeyPair:keyPairAttributes completionHandler:^(OSStatus status) {
          SecKeyRef privateKey = [keypairManager getPrivateKeyWithKeyTag:currentlLoginEmailAddress prompotMessage:@""];
          if (privateKey != nil) {
        
            NSData * publicKeyData = [keypairManager getPublicKeyWithKeyTag:currentlLoginEmailAddress];
            
            SubjectDN * subjectDN = [[SubjectDN alloc] init];
            subjectDN.commonName = userName;
            subjectDN.serialNumber = randomNumber;
            subjectDN.emaiAddress = currentlLoginEmailAddress;
            NSString * csr = [[CertificateManager getInstance] generateCertificateSigningRequestWithSubjectDN:subjectDN publicKeyData:publicKeyData privateKey:privateKey];
            if (csr.length > 0) {
            NSLog(@"%@",csr);
            }
        }
    }];

The Final output will be CSR in string form and copy and past into https://certlogik.com/decoder/ to check output
