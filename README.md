Langkah Awal
1. system > reset configuration > ✔️No Default Configuration > ✔️ Do Not Backup > Reset Configuration

2. system > identity > Irfan Syarifudin

3. system > user > + > name: irfan, group: full, password:i12345678 > login irfan

4. system > user > admin > - (remove)

5. Interface > ganti nama sesuai soal > e1(internet), e2(lokal), wlan1(wirelles)

Jaringan Internet
6.4. IP > DHCP Client > + > Interface: e1-i > apply > OK 

7.5. IP > Firewall > NAT > + > Chain: srcnat > Out Interface ether 1 INTERNET > Action > masquerade > apply > OK

Konfigurasi Wifi Router
8.1. IP > DNS > ✔️ Allow Remote Request > apply > OK

9.2. System > SNTP Client > ✔️ Enabled > pr: 0.id.pool.ntp.org >sc: 1.id.pool.ntp.org // New Terminal > ping >copas IP \\ > Apply > OK > System > Clock

20.3. Winbox > IP > web proxy > ✔️ Enabled > Cache Administrator: irfan_syarifudin@smkn3yogyakarta.sch.id > ✔️ Cache On Disk > Apply > OK

Jaringan Lokal
10.6. IP > Address > + > interface:e2-l > 192.168.100.1/25 > Apply > OK

12.7. IP > DHCP Server > DHCP Setup > e2-l > Next ⛔ > Addresses Give Out: 192.168.100.2-192.168.100.100 > Next  

14.8. IP > Firewall > Filter Rules > + > chain:input >src addres: 192.168.100.2-192.168.100.50 > dst.addres: IP Router (ether 1 INTERNET) > protocol: icmp > Action > action: drop > apply > OK;
CMD ORI > ping ip_router

15.9.  IP > Firewall > Filter Rules > + > chain:input >src addres: 192.168.100.51-192.168.100.100 > dst.addres: 192.168.200.1 IP Router (Wlan1 WIRELESS) > protocol: icmp > Action > action: drop > apply > OK;
CMD ORI > ping 192.168.200.1

14/15.8/9. IP > DHCP Server > Leases > 2x Klik > Make Static > OK; 2x klik > Addres:192.168.100.xx > apply > OK; Disable/Enable Ethernet

16.10. IP > Firewall > Filter Rules > + > chain:input > Action > action:log > log prefix: Tersimpan di disk > apply > OK ;
System > Logging > Action: disk > apply >ok;
Log > cek buffer (disk)

Jaringan Wireless
11.11. IP > Address > + > interface:w1-w > 192.168.200.1/24 > Apply > OK

13.13. IP > DHCP Server > DHCP Setup > w1-w > Next ⛔ > Addresses Give Out: 192.168.200.2-192.168.200.100 > Next 

17.12. Wireless > wlan1-wireless > Wireless > mode: ap bridge > band:B/G/N >SSID: Irfan_Syarifudin@ProxyUKK > apply > OK

18.14. IP > Hotspot > Servers > Hotspot Setup > HotSpot Interface: w1-w > Next ⛔ > DNS Name: irfan.net > Next > Name: admin, Password: i12345678 > Next;
Server Profiles > 2x clik irfan.net > Login > Login By: ✖️ Cookie > RADIUS > ✔️ Use RADIUS > Apply > OK;
RADIUS > + > Service: ✔️ Hotspot > Addres : 192.168.200.1 > secret: i12345678 > apply > ok;
Radius > Incoming > ✔️Accept > Apply > OK;
System > Package > harus ada user-manager;

19.15&21.14 ** User Manager > Routers > + > Shared Secret: i12345678 > Address: 192.168.200.1 (W-W1) > Apply > OK;
Settings > ✔️Enabled > Apply > OK;
Profiles > + > Apply > OK;
User > Add Batch > Number Of Users: 20 > Username Length: 3 > Username Characters: ✔️numbers > Password Characters: ✔️numbers > Profile: prof1 > Add Batch Users;
User Profiles > ctrl + a > Activate;
Limitations > + > Apply > OK;
Profile Limitations > + >From Time: 07:00:00 > Till Time: 16:00:00 > Apply > OK;  

Buat firewall yang memblokir
22.16. Web Proxy Settings > Access > + > Dst. Port: 80 > Dst. Host: www.linux.org > Action: deny > apply > OK 

23.17. Web Proxy Settings > Access > + > Dst. Port: 80 > Path: *mp3* > Action: deny > apply > OK; + > Dst. Port: 80 > Path: *mkv* > Action: deny > apply > OK; apply > OK

24.16&17. IP > Firewall > NAT > + > GENERAL > chain: dstnat > protocol: 6 (tcp) > Dst. Port: 80 > In.Interface: w1-w > Action: redirect > To Ports: 8080 > apply >OK; 
