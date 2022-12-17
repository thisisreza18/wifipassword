# wifipassword
find the WiFiPassword with Python programming language

import subprocess
import re

netsh_output = subprocess.run(
    ["netsh", "wlan", "show", "profiles"], capture_output=True).stdout.decode()

profiles_Name = (re.findall("All user Profile      : (.*)\r", netsh_output))

wifi_list = []

if len(profiles_Name) != 0:
    for name in profiles_Name:

        wifi_profile = {}

        profile_info = subprocess.run(
        ["netsh", "wlan", "show", "profiles", name], capture_output=True).stdout.decode()
        if re.search("security key           : Absent", profile_info):
            continue
        else:
         wifi_profile["ssid"] = name
        profile_info_pass = subprocess.run(
            ["netsh", "wlan", "show", "profiles", name, "key = clear"], capture_output=True).stdout.decode()
        password = re.search("key Content          :(.*)\r", profile_info_pass)
        if password == None:
            wifi_profile["password"] = None
        else:
             wifi_profile["password"] = password[1]
        
        wifi_list.append(wifi_profiles)

for x in range(len(wifi_list)):
    print(wifi_list[x])
