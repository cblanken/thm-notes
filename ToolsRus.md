# ToolsRus

web server: 80
login on `/protected`
user: bob, pass: bubbles

Told at login the /protected page has been moved to a different port

Appears to be unconfigured Apache Tomcat server on port 1234

We can login to _target_:1234/manager with previous creds
Same creds for _target_:1234/host-manager

Lab asks for nikto scan on /manager/html

