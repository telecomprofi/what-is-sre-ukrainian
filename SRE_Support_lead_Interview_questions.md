SRE Support Lead Interview questions
Intro - 5min

Areas we need to assess
Networking
ISO/OSI, TCP/IP, DNS, Subnetting/CIDR, tell me what happens when you type in ‘www.example.com’ into the browser in as much detail as possible (with protocols, calls, request/responses at Network, Transport, Application layers of ISO/OSI or TCP/IP layers) or which cli tools you would use to check issues at  network, transport, application layers.

OS Linux administration
cli, lsof, ps aux/zombie processes, top, Load Average,, view boot/ system logs, see open ports, disk usage, sudoers, how to add ssh keys to allow authentication to Linux host, find top 10 biggest files, chmod, chown, ln/inodes,  /tmp, ss, sysctl/service, cron, se, ipfw/chains

HTTP/S, REST API
Request Response, Methods (GET, POST), CDN cache hit/miss, Error codes, headers, how to check with curl, Postman, Web browser tools
Web app architecture - CNAME ->CDN-> A-record -> WAF -> LB -> FE instances -> LB -> BE instances -> DB cluster

Monitoring
Experience, Log centralized collection, Metrics how to get, traces, black box exporters/synthetic tests, alert thresholds, alert fatigue, how to send alert to Slack, best practice, observability, golden signals

Bash scripting
exit code, sed, awk, one-line to cut, replace config items with sed, the most complex script you’ve written, best practices

Incident Management
difference between Event, Incident, Problem. KPIs, your workflow, Priority: Impact vs Severity, MTTD, MTTR, First hit resolution, how to calculate Mean Time KPI (average), should outliers be included into mean? Percentiles
Checklists, runbooks, 
Tell me about the most complex tech incident you resolved/failed to resolve - what did you learn?

SRE
Key pillars, Concepts, SLI, Error Budget, Automation, RCA, Postmortems, HA/HL App Architecture,
Zero Downtime releases, Canary,  DDOS prot, Exponential Backoff, Rollbacks

Python scripting & Automation
Most complex script written Dunders, dictionary vs tuple vs list, pyenv, libraries used, 
Ansible, chef, puppet, RPA, etc

Clouds
Infra difference, what info is required to create Cloud infra from scratch in AWS/Azure/GCP (Resource Groups, IAM, VPC, AZs, Subnets, Security Groups, Public IP, DNS Record, xLB, VMs FE, VMs BE, DB, Storage, resource types: vCPUs, RAM, Disk IO, OS, Netw)

DB/SQL
Info required to connect to DB, connectivity check, Slow DB request investigation, indexing, Select/Join, Drop table, transaction logs, noSQL vs SQL, DB Migration when breaking schema change happens
Interview Questions
Technical:
Networking, OSI - ISO:
Q. At which level of ISO-OSI 7 Level model/TCP model does HTTPS (DNS/SSH) protocol work?  Which transport-level protocol  port number is used to send DNS requests/responses?
A: DNS protocol works at either Application level or 4th/Transport layer depends on point of view. 
A: UDP/Level4 & Port 53 
A: there is also TCP used by DNS over HTTPS(DoH) by most browser and DNS over TLS that use Levels 7-5 (HTTP/S)


Q: Which kind of DNS records are you familiar with and where you used them (A, AAAA, TXT, MX, CNAME)? How do you usually check for DNS issues with CLI?
A: 
A - resolving FQDN into IPv4
AAAA - resolving FQDN into IPv6
MX - Mail Exchange - resolving Mail server address into IP address
PTR - pointer, reverse lookup of IP to FQDN, for email security
TXT - text - used for email spam prevention, domain ownership verification, and framework (SPF) policies
A: dig <rec type> <Name server> <fqdn>
A: nslookup fqdn
A: on-line services like Google toolbox  https://toolbox.googleapps.com/apps/dig/

OS Linux
Q: How to check startup log on Linux?
A: dmesg, cat /var/log/boot.log, /var/log/dmesg, journalctl -b on systemd distros.
Q: How to check print/set/ env variables in Linux/*niox?
A: all env variables: printenv OR env, cat .bashrc | grep export, echo $varname"
A: export $varname=’Value’
Q: Which command do you use to see disk usage? What does -H option usually mean? -v?
A: df -H du -H
A: -H means human-redable
A: -v means verbose

Q: What does Load Average in ‘top’, ‘uptime’ command show?
A: Average Number of  processes that use CPU/Memory/IO/Network requests that wait in the kernel queue during the last 5/10/15 minutes?
Generally, a load average of less than the number of CPU cores is normal, as it means there are enough resources for all processes to run smoothly.
E.g machine with 8 CPU cores is fully loaded if  LA is 8 8 8 

Q: How to do real-time printing of log file content and filter only lines with ‘ERROR’ keyword inside?
A: tail -f /var/log/messages/ | grep ‘ERROR’

Q: What is inode ? how to check its usage?
A: df -I 
A: The inode (index node) is a data structure in a Unix-style file system that describes a file-system object such as a file or a directory. Each inode stores the attributes and disk block locations of the object's data.

Q: What happens when there are no Inodes left?
A: No new files could be created/saved. Reboot - login in RO mode and delete files, increase inode number in systct limits/ulimit
Q: How do you change file access rights in Linux
A: sudo chmod

Q: What is zombie process in Linux?
A:

Q: How to provide access to Linux machine with SSH keys?
A: manually copy public key to ~/.ssh/authorized_keys  OR ssh-copy-id username@remote_host
Also some config on disabling password authentication in sshd.conf and chmod 664 id_rsa.pub


OS: Windows
Q: How to list all env variables in CMD/Powershell?
A: ‘set’
Q: What is the difference between Powershell Administrator mode and user mode?

Q: What are OU in Active Directory?
A:
Q: Can object exists in multiple OUs in the Active Directory.
A: NO, a user cannot exist in two different OUs in an Active Directory domain at the same time. A user can be moved from one OU to another, but at any one point in time, it only resides in ONE location.
Q: Differences between Active Directory groups and organizational units:
A: Organizational units are used to delegate admin rights and apply group policy settings, while groups are used to manage permissions – for example NTFS permissions and share permissions on the file server or mailbox permissions in Exchange

Q: How to get list of services running from CMD?
A:  ‘net start’ or ‘sc queryex type=service state=all’
A: ‘Get-Service’, ‘gsv’
Q: What is equivalent of grep in Powershell/cmd?
A: ‘findstr’
Q: How to get list of Event Logs available? 
A:Get-EventLog -List
Q: How to get recent event from System EventLog?
 Get-EventLog -LogName System -Newest 5
Q: How to install service from PowerShell? Same done remotely? With Microsoft System Center Configuration Manager, System Manager SCCM/Endpoint manager,  Systems Management Server  (SMS)?
A: 
Scripting:
Bash, PowerShell
Q: Which scripting language you are the most familiar with?
Bash: 
Q: How to check script exit code? What do different exit codes mean?
A: echo $?
A: 0 - success, other - errors

Q: Bash: how do you reference positional arguments inside bash script (#script.sh ‘Kobzar’ ‘37’, ‘Taras Hryhorovych Shevchenko’?
A: ‘$1’, ‘ $2’ etc
Ex: echo "Username: $1"; echo "Age: $2"; echo "Full Name: $3";

Q: Passing input to bash script using flags, e.g.: userReg-flags.sh -f 'John Smith' -a 25 -u john
A: while getopts u:a:f: flag
do
    case "${flag}" in
        u) username=${OPTARG};;
        a) age=${OPTARG};;
        f) fullname=${OPTARG};;
    esac
done
echo "Username: $username";
echo "Age: $age";
echo "Full Name: $fullname";


Q: Difference between $* and $@
A: $* is a single string, whereas $@ is an actual array
Q: Describe the most complex script you have written (what did it do? libraries use)











What is SRE for you? Four Golden Signals, SLO/SLA
Are you familiar with  ITSM/ITIL MTTR/MTTD?

Observability/Monitoring - experience (FluentD, PromQL, SQL requests, agent-based, APM, ELK - Kibana requests, )?
Q: Can you explain what steps are required to:
Q: Configure server/VM monitoring with NewRelic/DataDog?
A: Install Infra agent, configure it with .yaml configuration file, check CPU/Mem/Disk/IO/Network  metrics are flowing in SaaS UI (Infrastructure section -> search for hostname -> check if there is data on standard Infra dashboards).
Q: Create Alerts for 85% CPU over 5-10minutes? Is there better way to do it?
A: Write (NRQL for NR) request for specific metric and set threshold for alert at 85% over period of 5-10 minutes.
A: It is wise to use Anomaly Detection than static thresholds, it makes sense to configure such alert with Terraform in Observability as a Service approach when adding any new VM/Server to monitoring. Also good practices are: - documenting Infra if it is static, or add Tags/metadata when it is dynamic (Scale out, scale in), - testing if alerts are not too chatty, - define cool down period to avoid ‘flapping’ alerts
Q: Collect logs from specific application with NewRelic/DataDog/FluentD? (logstash/fluentbit?)
A: Edit configuration yaml and add 1) path to the log file, 2) file format, 3) filtering/exclude/grep conditions, 4) log forwarding destination (e.g. address:port:Encryption options of Log server/Load balancer, or intermediate Log aggregation server (client -> regional log aggregator -> central/distributed Log Server).
Q: Collect metrics from specific applications via Push or Pull? Which is better for distributed apps? How to improve/automate the process of adding/removing scraping endpoints?
A: Pull - configure Centralised collector with endpoint of the specific web-app instance that exposes metrics (e.g. https://web-app:443/metrics and frequency of polling, pulling (e.g every 1 min, 30sec etc). Configure metric filtering to collect only specific
A: Push: configure app with Metric Ingest API endpoint where app has to send POST API request with its metrics, App has to implement retry with exponential back-off logic, batches, track limit of KB per POST message etc, limit number of API request per minute to avoid DDOS (300k per hour for NR)
Q: Collect APM data from web applications? What is APM?
A: Dev has to implement collection of variables (number of active connection, Errors, Latency of requests/responses, mem usage, CPU usage, queue wait time, success/failed request attempts, app uptime, etc) and send that data with Library (Instrumentation to SaaS) for dashboards, alerts, telemetry. JVM/.NET and similar frameworks implement telemetry as separate process that could be queried with APM agents
       f)  Q: What is the difference between RUM and APM?
       g) Q: Collect call traces of distributed applications/microservices? What is the span? 
       d) Q: Monitor web-service SLI that has exposed ‘https://webservice:443/health’ endpoint?
            A: curl with GET/POST, Postman, Any other API tool, synthetic tests (NR,DataDog), BlackBox Exporter with Prometheus. If response is 200 - OK within 3 sec response time from at least two regions - Available, other types of HTTP response or takes longer, or not response from at least two locations - down/alert. Similar to the Ping test in NR. Could be combined with SSL certificate expiration requests.

Public cloud:
Q: Azure Cloud - what is Resource Manager Group, What is required to create VM (network, compute, storage, Load Balancer, Auto Scaling group)
Q: Any IaaC tool used to create infrastructure on Azure? What are your best practices?
A: terraform, ARM templates, bicep, Pulumi, etc. BP: to avoid config drift disallow changes via UI, only through code. Test IaaC before/on every release (create/destroy new env with it). Lock changes with lock/save state on cloud storage/no sql db (or hashicorp cloud).
Test changes with new IaaC provider releases. Maintain modules centrally. Test IaaC with tools like terragrunt, snyk, trivy, terradocs, trivy, checkov for security anti-patterns. Automate PRs with Atlantis, terraform cloud. Scan your public/private infra with pen-test tools regularly. Update CMDB of scan tools with automatic service discovery.
A: What are your Azure/Cloud best practices?
Q: tag everything, report grouped per tags (app:, env: team: component: ), separate resource groups per app, keep DB, backend in internal VPCs, Load-balancers & FE in external VPCs, Scale down when there is no load.Document Security groups, IAM groups Azure AD IAM, well-architected framework, redundant instances in at least 2 AZs for production. Geo-redundancy, WAF, CDNs, Active-active for DB stuff, tiered blob-storage,  use, expose only specific ports with security groups, scan exposed public IPs, monitor with SaaS tools, set alerts, collect logs, audit logs ingest into SIEM, track unused resources - shut down for scream test. Policies/Guardrails.

Q: Cost saving - how do you decrease the cost of Public Cloud Infra?
A: e.g. Track your resource group’s _estimated_ usage and set trend alert for $$$. Or use infrcost/cubecost to estimate costs before applying IaaC code. Use spot instances or B-grade VMs instead of on-demand for batch jobs, worker nodes, low CPU stuff. Use hardware licenses on Dedicated hosts (hybrid).
Q: AWS Well-Architected Framework, six pillars
A: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability. Provides guidance to help apply best practices in the design, delivery, and maintenance of AWS workloads.


Linux/*nix/MacOS CLI:
Q: how to troubleshoot DNS/TCP/UDP connection/scan for open ports/ tail search logs, list of open files per process, capture network traffic, resource usage ?
A: dig, traceroute, ss/netstat lsof, awk, sed, grep, tail, tcpdump, top/
Q: what is heredoc in bash? what is jinja2 used for? what double curly brackets is  used for {{}} ?
Q: what is /cat/proc/ directory on unix/linux?
Q: how to see childs of the process, how to install binaries in ubuntu or rhel?
Q: how to kill process gracefully?

DB SQL
SQL server experience? NoSQL? 
Q: How to check connectivity to SQL DB?
A: use: nmap + db endpoint address:port to see if TCP connection is open. Then use sql client and DB endpoint address:port with login/password  to log-in. Then check with a simple request like ‘select 1 from users’ to get number of rows in table ‘users’ or similar?

Q: What to do when DB host’s disks are full?
A: compact db, delete transaction log

Q: What is DB transaction log? How do you delete it?
A: things that records transactions and could be re-applied when DB crashes.

Q: What is db migration during release when schema changes? What are best practices? (Block updates/lock tables -> Backup, keep Read Replicas up-> Restore if DB corrupted or Rollback is required).
A: When there are breaking DB schema changes during release (tables, fields are added or deleted/renamed) there is a need to apply these changes (and/or update/populate rows). There are migration frameworks (e.g. SQL alchemy for Python, 

Centralised Version control/GIT

Alert Management (PagerDuty, etc)
Azure DevOps pipelines, CI/CD
E-com, OMS
ERP/
ServiceNow Incident Management
Docker/K8s 

SRE
ITSM/ITIL MTTR/MTTD
Observability/Monitoring
Azure DevOps pipelines
Docker/K8s
Azure Cloud
IaaC
Scripting
Architecture/HA/clusters:

Load balancing:
Cheapest available - DNS
L4, L7 - what is the difference between
Round robin vs wehighted/load based balancing
what is difference between graceful restart and kill of process?
Drain connections/Slow start?
what is a rolling restart in a cluster?
what are sticky session in http calls?
what is stateless service? 
what are 12 factor apps?
what is immutability?
what is affinity/anti affinity in distributed apps?
what are heartbeats in cluster app instances and how they are used during scaling/issues with application instances? What other ways to scale/restart app instances?


what are ‘snowflake’ VS ‘cattle’ approach  in infrastructure management?

Managerial/PMI:
Tell me about your experience building a 24x7 team and your role? Team structure/Seniority, how long did it take to hire? Which Engineering profile you were after? What were the biggest challenges?
How would you structure initial onboarding of new Shift Engineers?
How would you organize knowledge transfer? 
How long in your opinion does it take for a team to start operating 24x7?
How would you understand if a team is performing good or bad?
What are key challenges in implementing shift work?
Any best practices in this area (24x7 Application support) you would like to implement/make sure it is there?
Which tools do you believe shift engineers would need in their daily work?
Tell me about cases when you/your team failed - what did you learn from them?
How do you learn new things? What you have learned last year/month?
Anything taks/project you are especially proud of?
What makes you quit/consider leaving?
What are your personal goals for the next few years?





