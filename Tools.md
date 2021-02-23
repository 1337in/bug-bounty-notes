# Tools 

## Summary
- [Tools](#tools)
  - [Summary](#summary)
    - [Subdomains Enumeration](#subdomains-enumeration)
    - [archived data gathering](#archived-data-gathering)
    - [CSRF POC GENERATOR](#csrf-poc-generator)


### Subdomains Enumeration
- [Sublist3r](https://github.com/aboul3la/Sublist3r)
  
  Sublist3r is a python tool designed to enumerate subdomains of websites using OSINT. It helps penetration testers and bug hunters collect and gather subdomains for the domain they are targeting. Sublist3r enumerates subdomains using many search engines such as Google, Yahoo, Bing, Baidu and Ask. Sublist3r also enumerates subdomains using Netcraft, Virustotal, ThreatCrowd, DNSdumpster and ReverseDNS.
- [Knockpy](https://github.com/guelfoweb/knock)

  Knockpy is a python tool designed to enumerate subdomains on a target domain through a wordlist. It is designed to scan for DNS zone transfer and to try to bypass the wildcard DNS record automatically if it is enabled. Now knockpy supports queries to VirusTotal subdomains, you can setting the API_KEY within the config.json file.
- [assetfinder-0.1.0](https://github.com/tomnomnom/assetfinder) 
 
  a tool for gathering subdomain
- [PassiveHunter](https://github.com/devanshbatham/Passivehunter)

  Passivehunter uses `https://dns.bufferover.run` for enumerating subdomains , This project uses the The Rapid7 [Project Sonar](https://opendata.rapid7.com/) datasets . dns.bufferover.run uses DNSGrep for quickly searching the the large data sets , Passivehunter enumerates the subdomains using query `https://dns.bufferover.run/dns?q=<hostname>`. It uses some regex magic to filter out the subdomains from the raw json output , then all the alive subdomains are filtered. It shows the status code of the alive subdomains. It is fast as it uses `asynchronous` requests instead of traditional `synchronous` requests.


### archived data gathering

- [ArchiveFuzz](https://github.com/devanshbatham/ArchiveFuzz)

  ArchiveFuzz hunts down the archived data(subdomains/emails/API keys) of domains. Web archiving is the process of collecting portions of the World Wide Web to ensure the information is preserved in an archive for future researchers, historians, and the public.

  This tool uses webarchive's cdx to enumerate Emails/Subdomains/IPs/API tokens .

### CSRF POC GENERATOR

[HTML CSRF POC GENERATOR](https://github.com/merttasci/csrf-poc-generator)