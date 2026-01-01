1. Objective

   Reverse engineer the signature algorithm of the [App Name] (China Telecom App) using Frida and implement an active call (RPC/Function Invocation) to the signature method.

3. Environment & Tools

   Operating System: macOS

   Decryption: frida-ios-dump (for IPA decryption/dumping)

   Traffic Analysis: Reqable Proxy

   Hardware: Jailbroken iOS device (for dumping and dynamic debugging)

   Static Analysis: IDA Pro

5. Analysis Process

   Phase A: Traffic Interception (SSL Unpinning)

   Initial Observation: When attempting to capture traffic, the following error occurs:

   "The certificate for this server is invalid. You might be connecting to a server that is pretending to be 'appgologin.189.cn'..."

   Critical Step: SSL Pinning must be bypassed before the application's network traffic can be inspected.

   Target API (Login Example): https://appgologin.xxx.cn:xxxx/login/client/userLoginNormal

<img width="3482" height="1742" alt="image" src="https://github.com/user-attachments/assets/26b53dec-eefc-42fc-83fb-408947aa75b9" />

   Login Packet Parameter Analysis
   
   The parameter loginAuthCipherAsymmertric is fundamentally based on the RSA algorithm.

   B. Algorithm Analysis
   
   Through static analysis in IDA Pro, the call hierarchy of the RSA algorithm functions is identified as follows:

   +[Utils createRSAStringWithPhone:authentication:timestamp:slidingTime:percentage:]

   Purpose: High-level wrapper that collects user data and prepares it for encryption.

   +[RSAEncryptor encryptString:publicKey:]

   Purpose: Converts the input string into a data format suitable for the encryption engine.

   +[RSAEncryptor encryptData:publicKey:]

   Purpose: The core encryption logic that performs the RSA transformation using the public key.

   ......

