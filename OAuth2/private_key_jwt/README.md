OPENIG-1412 private_key_jwt authentication (OIDC)
======
> In a nutshell, with **private_key_jwt** authentication, the client sends a signed JWT to authenticate to the Authorization Server.<br>
> The JWT is forged by the client and must contain specific claims (iss, sub, aud) as described in [OpenID RFC](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication). <br>
> The JWT is then signed by the client with its private key.<br>
> When the client authenticates to the Authorization Server, this AS checks for the validity of the signed JWT <br>
> using the public key that the client entered first on its registration on the server.
<br>

**STEP-1**: Install and configure [connect2id server](https://connect2id.com). <br>
            Don't forget to modify your connect2id-server-6.3/tomcat/webapps/c2id/WEB-INF to:<br>
                 - Allow dynamic client registration: **op.reg.allowOpenRegistration = true**<br>
                 - Allow non TLS redirections: **op.reg.rejectNonTLSRedirectionURIs = false**<br>
            
**STEP-2**: Open and EDIT the groovy script configuration to configure your OpenIG with your Connect2Id. Then launch it with:<br>
        '`$ groovy connect2id.groovy`'
        <br> It also creates a 'openig.properties' in your `user.home` directory used by the json configuration files.
        <br> Copy the generated private key which should be displayed on the screen after the execution of the script.
  
**STEP-2**: Backup your home folder '.openig'. <br>
            Copy the provided folder '.openig' to replace it.
            Paste the private key in the `connect2id_private_key_jwt.json` file.

**STEP-3**: Launch OpenIG and check on the url '<openig-url>/openid'
        <br> example: `http://openig.example.com:8082/openid`
        <br> You should be redirected to log on connect2id.
        <br> Enter alice/password -> You are then authenticated to the protected resource: success!
        <br>
> **Behind the scenes**: OpenIG forges the JWT using the data declared in the ClientRegistration object, <br>
> and complete the **iss**, **sub** and **aud** claims. Then, it adds **iat**, **jti** and **exp** claims <br> 
> which are computed on the fly. <br>
> It signs the JWT and perform the **private_key_jwt** authentication to Connect2Id.<br>
> On success, you are allowed to access to the protected resource.<br>
  
![ForgeShop logged in using private_key_jwt](https://raw.githubusercontent.com/openig-contrib/script-util-for-openig/master/media/logged_in_private_key_jwt.png)
 


----------
* [See also: OPENIG-1412](https://bugster.forgerock.org/jira/browse/OPENIG-1412)

----------

**Configuration** : OPENIG-5*


Disclaimer
=============

This project is not supported by ForgeRock AS.
