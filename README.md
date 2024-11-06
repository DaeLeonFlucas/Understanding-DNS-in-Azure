
<h1>Understanding DNS</h1>
In this lab, we’ll examine the practical use of DNS, a core concept in IT. While many sources cover DNS theory, here we’ll configure DNS records to see how they work firsthand. This lab continues from a prior setup where a client is connected to my domain ernestotest.com. I am logged in on both the client and domain controller VMs as Jane Doe, using an admin account. An administrator login is necessary for this lab to function as expected. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>DNS Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/5pjgtVz.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/Zl04Jyt.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/96xUgLF.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
While logged into the client machine as an admin, open Command Prompt. When you try to ping "mainframe," the attempt will fail, and using nslookup mainframe will show no DNS record for it, indicating that a DNS A-record is needed for "mainframe" on the domain controller.
To create this record, go to the domain controller, open DNS Manager, and navigate to the domain you set up in the Forward Lookup Zones tab (e.g., ernestotest.com). Right-click in this domain space and select New Host (A or AAAA). Enter "mainframe" as the name, and set the IP address to match the domain controller’s IP, which will allow ping requests to resolve correctly.
After setting up the record, refresh the DNS server to apply the update. Back on the client machine, attempt to ping "mainframe" again, and you should now see the hostname resolve successfully.
</p>
<br />

<p>
<img src="https://i.imgur.com/nLlOGKl.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/p0VcajB.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/ds0SCKW.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/d4cXTCS.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
This exercise will demonstrate how the DNS cache works. On the domain controller, update the IP address of "mainframe" in the DNS record to 8.8.8.8 (Google's public DNS IP) and refresh the DNS server.
Then, return to the client and try to ping "mainframe" again. You'll notice that it still pings the original IP address. To investigate why, use the command ipconfig /displaydns, which will reveal that the client’s DNS cache still holds the old IP.
To force an update, clear the DNS cache by running ipconfig /flushdns. This command will empty the cache, allowing the client to receive the updated IP address. Now, when you ping "mainframe" again, you should see the new IP address, 8.8.8.8, reflecting the recent change in the DNS record.
</p>
<br />

<p>
<img src="https://i.imgur.com/rI2NYU7.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/FmVM0IU.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/dyWduSW.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
We will configure a CNAME record on the DNS server to map "search" to Google. Access the domain tab in the Forward Lookup Zones section of the DNS Manager and create a new CNAME record labeled "search," pointing it to Google. Once the server is refreshed, testing with a ping and nslookup from the client will display the corresponding CNAME record.
</p>
<br />

<h2>Lessons Learned </h2>

DNS records can change over time, and this can cause connectivity problems if the cache contains outdated information. The ipconfig /flushdns command, a frequent reference in IT troubleshooting, was something I didn't fully understand until this lab. In the context of pinging "mainframe" at the beginning of the lab, the DNS cache was checked first. If no result was found, the host file was consulted, and finally, the DNS server was queried. By adding a record to the DNS server, I was able to resolve the ping to "mainframe." Additionally, a CNAME record is used to create an alias for a domain, such as how "search" was aliased to Google in this lab.
