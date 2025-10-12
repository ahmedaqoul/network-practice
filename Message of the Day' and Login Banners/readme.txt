1. Use PC1 to connect to R1 via the console port.

2. Create two users on R1:
   Username: ccna / Secret: Cisco
   Username: ccnp / Secret: Cisco

3. Set the console port to use the local database to authenticate users.

4. Set a 'message of the day' banner of 'Welcome to Packet Tracer' and a login banner of 'Authorized users only'.

5. Logout and then login with one of the users created. Did the banners display properly?


lab devices : 

1. 1 router 
2. 1 pc

lets make the first step

1. 1. Use PC1 to connect to R1 via the console port.

from pc chose the 'RS 232' and from router way chose 'console'

lets click on pc1 > Desktop > terminal and press ok to accept default configuration
__________________________________________________

Now step 2

2. Create two users on R1:
   Username: ccna / Secret: Cisco
   Username: ccnp / Secret: Cisco

Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#username ccna secret Cisco
Router(config)#username ccnp secret Cisco

'secret' its make your password encrypted 

write 'do show run' to check

username ccna secret 5 $1$mERr$YlCkLMcTYWwkF1Ccndtll.
username ccnp secret 5 $1$mERr$YlCkLMcTYWwkF1Ccndtll.

____________________________________________________

Now step 3 

3. Set the console port to use the local database to authenticate users.

Router(config)#line console 0
Router(config-line)#login local

- When you set the console port to use the    local database for authentication, it means the router will require a username and password stored locally before allowing access through the console.
This enhances security by ensuring only authorized users saved in the router’s local database can log in.

____________________________________________________

now step 4 

4. Set a 'message of the day' banner of 'Welcome to Packet Tracer' and a login banner of 'Authorized users only'.


Router(config)#banner motd ?
  LINE  c banner-text c, where 'c' is a delimiting character


Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#banner motd ?
  LINE  c banner-text c, where 'c' is a delimiting character
Router(config)#banner motd *welcome to packet tracer*	
Router(config)#banner login *Authorized users only!*


____________________________________________________

step 5 Logout and then login with one of the users created. Did the banners display properly?


Router(config)#end
Router#logout

Username: ccna
Password: 

Router>