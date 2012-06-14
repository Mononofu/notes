to fix the insufficient permissions for device error on ubuntu, do this as root:

adb kill-server 
adb start-server

adb devices

should then list all connected devices