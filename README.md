1. Objective
Reverse engineer the signature algorithm of the [App Name] (China Telecom App) using Frida and implement an active call (RPC/Function Invocation) to the signature method.

2. Environment & Tools
Operating System: macOS

Decryption: frida-ios-dump (for IPA decryption/dumping)

Traffic Analysis: Charles Proxy

Hardware: Jailbroken iOS device (for dumping and dynamic debugging)

Static Analysis: IDA Pro

3. Analysis Process
Phase A: Traffic Interception (SSL Unpinning)
Initial Observation: When attempting to capture traffic, the following error occurs:

"The certificate for this server is invalid. You might be connecting to a server that is pretending to be 'appgologin.189.cn'..."

Critical Step: SSL Pinning must be bypassed before the application's network traffic can be inspected.

Target API (Login Example): https://appgologin.xxx.cn:xxxx/login/client/userLoginNormal

<img width="3482" height="1742" alt="image" src="https://github.com/user-attachments/assets/26b53dec-eefc-42fc-83fb-408947aa75b9" />

图中登录封包参数：

loginAuthCipherAsymmertric的本质是rsa算法！


 

b.算法

ida pro 中静态分析，rsa算法函数调用层次如下：

1.+[Utils createRSAStringWithPhone:authentication:timestamp:slidingTime:percentage:]

2.+[RSAEncryptor encryptString:publicKey:]

3.+[RSAEncryptor encryptData:publicKey:]

......

