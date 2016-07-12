***TCPDUMP*** 

Tcpdump Unix benzeri işletim sistemlerinde komut satırından çalışan genel bir paket analizcisidir. Bilgisayara gelen veri paketlerini kaydetmeye, incelemeye, filtremeleye yardımcı bir sistemdir. Kullanıcıya bağlı bulunduğu bir ağ üzerinden iletilen veya alınan TCP/IP paketlerini veya diğer paketleri yakalama ve gözlemleme olanağı sunar. BSD linansı altında dağıtılan tcpdump ücretsiz bir yazılımdır. 
Tcpdump paket yakalamak için lilpcap kütüphanesini kullanır.
Tcpdump, paket yakalama işlemini bitirdikten sonra, yakalanan paket sayısı ile ilgili bir rapor sunar. 
Tcpdump kullanılarak yapılacak paket yakala işleminde eğer bir filtreleme uygulanacak ise bu durumda Berkeley Packet Filter (BPF) ifadelerini kullanırız. Tcpdump, Windump, Wireshark ve gibi bir çok sniffer uygulaması BPD ifadelerini kullanarak filtre tanımı yapılmasına izin verir.

***TCPDUMP TARİHİ***

İlk olarak 1987 yılında Van Jacobson, Craig Leres ve Steven McCanne tarafından Lawrence Berkeley Laboratory Research Group da çalışırlarken yazıldı. 1990'ların sonuna doğru çeşitli işletim sistemlerinin bir parçası olarak çok sayıda tcpdump versiyonu ve iyi düzenlenmemiş birçok yaması dağıtıldı. Michael Richardson ve Bill Fenner 1999 yılında tcpdump.org oluşturuldu. 

Tcpdump ilk olarak Unix sistemler için yazılmış olsada daha sonra Windows'a port edilmiş ve *Windump* olarak adlandırılmıştır.



***TCPDUMP BİLGİSAYARA YÜKLEME***

Bazı linux dağıtımlarda kurulmuş olan tcpdump programını komut satırına aşağıdaki komut yardımıyla bilgisayarımıza kurabiliriz.
**sudo apt-get install tcpdump**

 Windows için ise http://www.winpcap.org adresindeki winpcap kütüphanesi indirilmeli ve sonrasında http://www.winpcap.org/windump adresinden Windump indirilerek yüklenebilir.
 
***TEMEL PARAMETRELER***
Temel parametrelere geçmeden önce;

 - Temel kullanımı
	 `#tcpdump -i <Arayüz> <filtreleme>`
 - Linux/Unix sistemlerde *tcpdump* komutunu kullanabilmek için ya root haklarına sahip olmak lazım ya da tcpdump komutu öncesinde sudo ifadesi eklenmeli.
 - Tcpdump komutunu, BPF komutları ile birlikte kullanılarak filtreleme işlemleri yapılabilir.

	    #tcpdump -i wlan0 port 80 //Port numarası 80 olan paketleri yakalar

 - Mantıksal (and ( && ), or ( || ), not ( ! ) ) ifadeler kullanılarak birden fazla filtreleme işlemi yapapılabilir.
 `#tcpdump -i wlan0 not arp and icmp`

***-A ==>*** Yakalanan paketlerin içeriğini ASCII formatında ekrana basar.
![enter image description here](http://www.teknikblog.org/wp-content/uploads/2015/08/3.png)

***-c ==>*** Belirtilen sayıda paket yakalaması yapar
![enter image description here](http://www.teknikblog.org/wp-content/uploads/2015/08/2.png)
***-C ==>*** Paketleri belirli boyutlarda kayıt etmek istenildiğinde kullanılır

    tcpdump i wlan0 -w kayit.pcap -C 100 // Paketleri 100 Mb'lık dosyalar halinde kayıt ederek tarama işlemine devam eder.
    
***-D ==>*** Sistemde kullanılabilir olan ağ arayüzlerinin listesini verir.

    # tcpdump -D
	1.eth0
	2.eth1
	3.eth1.780
	4.any (Pseudo-device that captures on all interfaces)
	5.lo

***-e ==>*** Paketlerde bulunan Mac adreslerini elde etmek için kullanılır.

    tcpdump -i wlan0 -e
	14:56:45.450574 04:18:d6:ce:06:c1 (oui Unknown) > Broadcast, ethertype IPv4 (0x0800), length 185: 192.168.1.200.46608 > 255.255.255.255.10001: UDP, length 143

***-E ==>*** Bir şifreleme anahtarı tarafından IPSEC trafiği şifrelenir.
***-i ==>*** Belirtilen ağ arayüzü üzerinden dinleme yapar.

    tcpdump -i wlan0

***-n ==>*** Dns isimleri çözümlemesi yapılmaz.

    # tcpdump -n -i eth0
	tcpdump: verbose output suppressed, use -v or -vv for full protocol decode listening on eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
	12:07:03.952358 IP 172.16.25.126.ssh > 172.16.25.125.apwi-rxspooler: Flags [P.], seq 3509512873:3509513069, ack 3652639034, win 18760, length 196
	12:07:03.952602 IP 172.16.25.125.apwi-rxspooler > 172.16.25.126.ssh: Flags [.], ack 196, win 64171, length 0
	12:07:03.953311 IP 172.16.25.126.ssh > 172.16.25.125.apwi-rxspooler: Flags [P.], seq 196:504, ack 1, win 18760, length 308
	12:07:03.954288 IP 172.16.25.126.ssh > 172.16.25.125.apwi-rxspooler: Flags [P.], seq 504:668, ack 1, win 18760, length 164
	12:07:03.954502 IP 172.16.25.125.apwi-rxspooler > 172.16.25.126.ssh: Flags [.], ack 668, win 65535, length 0
	12:07:03.955298 IP 172.16.25.126.ssh > 172.16.25.125.apwi-rxspooler: Flags [P.], seq 668:944, ack 1, win 18760, length 276
	12:07:03.955425 IP 172.16.23.16.netbios-ns > 172.16.31.255.netbios-ns: NBT UDP PACKET(137): REGISTRATION; REQUEST; BROADCAST
	12:07:03.956299 IP 172.16.25.126.ssh > 172.16.25.125.apwi-rxspooler: Flags [P.], seq 944:1236, ack 1, win 18760, length 292
	12:07:03.956535 IP 172.16.25.125.apwi-rxspooler > 172.16.25.126.ssh: Flags [.], ack 1236, win 64967, length 0

***-nn ==>*** Yakalanan paketlerde dns, port, ve protokol çözümlemesi yapılmaz.
***-q==>*** Sadece temel bilgilerini içeren bir analiz yapar.
***-p ==>*** Promisc moddan kaçmak için kullanılır.
***-r ==>*** Pcap olarak kayıt edişmiş dosyayı okur.

    tcpdump -r pcapadi.pcap

***-s ==>*** Her bir paketin ilk ne kadarlık kısmının yakalanacağını belirtir. Varsayılan olarak bu değer 68 byte'dır.
***-t ==>*** Yakalanan paketlerin zaman çıktısı olmadan görüntüler.

    # tcpdump –t 
	2.174.108.162.29157 > cc.huzeyfe.net.2000: P 3329:33 81(52) ack 11236 win 17520 (DF) [tos 0x20] 
	cc.huzeyfe.net.2000 > 2.174.108.162.29157: . ack 22 89 win 8576 (DF) [tos 0x10] 

***-v ==>*** Paketler hakkında daha fazla bilgi verir
***-vv ==>*** Paketin NFS ve SMB içeriğini de gösteren detaylı bir analiz yapar.
***-vvv ==>*** Paketlerin Telnet içeriğini de gösterir.
***-w ==>*** Yakalanan paketleri dosyaya yaz.`

    #tcpdump -i wlan0 -w Yakalananpcaplar.pcap

***-x ==>*** Çıktıyı HEX olarak yazdırır.
***-XX ==>*** Çıktıyı HEX ve ASCII olarak yazdırır.
![enter image description here](http://www.teknikblog.org/wp-content/uploads/2015/08/Terminal-root@daca-home-caner_002.png)

***Örnek Komutlar*** 

    tcpdump -i wlan0 -v -nn -w yakalanpaketler.pcap
Wlan0' dan geçen paketleri dns ve ip çözümlemesi yapmadan detaylı bir şekilde yakalananpaketler.pcap dosyası olarak kayıt eder.

***BPF (Berkley Packet Filter) İle Örnekler:*** 
BPF üç ana kısımdan oluşmaktadır.

***TYPE:*** Host, net, port   parametreleri. 
	

    tcpdump -i wlan0 port80

  Wlan üzerinde 80 nolu porttan geçen paketleri yakalar.

***Direction:*** Src, dst   parametreleri. 

    tcpdump -i wlan0 src 192.168.1.1 and dst 192.168.1.3

Ağ üzerindeki iki makine arasında TCP protokolü kullanarak yapılan paketleri yakalar.

***Protocol:*** Ether, fddi, wlan, ip, ip6, arp, rarp, TCP, UDP
	

     tcpdump -i wlan0 udp

Wlan üzerinden UDP paketlerini yakalamada kullanılır.

    tcpdump -i wlan0 dst 192.168.1.2 and port80

Wlan üzerinde hedef ip adresi 192.168.1.2 ve 80 nolu porta gelen paketleri yakalar.












***Kaynaklar : ***
 https://tr.wikipedia.org/wiki/Tcpdump
http://www.teknikblog.org/?p=410
