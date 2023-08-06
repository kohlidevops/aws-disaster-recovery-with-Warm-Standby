# aws-disaster-recovery-with-Warm-Standby

 Let's say you have a critical application that requires high availability. In a warm standby setup, you maintain a scaled-down version of your entire production environment, including servers, databases, and configurations. These resources are not fully operational but are ready to be quickly brought online. If a disaster happens, you can quickly launch additional instances, restore data, and apply configurations to get the application running with minimal downtime.

 Before start this method, Please deploy workloads using below link

https://github.com/kohlidevops/aws-disaster-recovery/blob/main/README.md

https://github.com/kohlidevops/aws-disaster-recovery-with-backup-restore-/blob/main/README.md

https://github.com/kohlidevops/aws-disaster-recovery-with-region-outage/blob/main/README.md

https://github.com/kohlidevops/aws-disaster-recovery-with-Pilot-Light/blob/main/README.md

Now Mumbai is Primary regions and NVirginia is Secondary region. Make ensure both ALB DNS working fine.

**To create a Health Check in Route53**

Navigate to AWS R53 console – select – Health check (Health check for Mumbai region ALB DNS)

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Warm-Standby/assets/100069489/e83f2b43-870e-46c5-9312-09c4c0cd51c1)

Finally, my health check has been created. (Its for Mumbai ALB)

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Warm-Standby/assets/100069489/78015f19-835d-497b-bcad-26bc2956090c)

**To update configure in Route53**

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Warm-Standby/assets/100069489/550af208-5fbb-41da-80f3-37c33b805328)

Record value should be Mumbai ALB DNS

Routing type should be Failover.

Failover record type – Primary.

Health check id – MumbaiHealthCheck (Just created now)

Record Id – Mumbai Load balancer.

Then update the record.

**To create CNAME record for NVirginia Load balancer**

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Warm-Standby/assets/100069489/5f69ba03-a110-4427-93b9-ffac34ba11fc)

To add some data again in application (currently mapped to Mumbai)

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Warm-Standby/assets/100069489/448044c7-1440-4210-8da1-1f413f385059)

Now Just make Mumbai ALB as not reachable through add NULL SG and remove ALB SG. SO no inbound and outbound rule. It shouldn’t reach. Its like Region outage we consider.

Now, not reachable.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Warm-Standby/assets/100069489/dd800f5d-1e1e-4bf6-b98a-c9d500e19651)

Health check showing, first 30 minutes its healthy which mean Mumbai region was working as we expected, then there were some outages. As a result, the Health check is Unhealth. Due to this Unhealthy signal, Route53 redirected traffic from Mumbai to NVirginia Load balancer using Failover routing.

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Warm-Standby/assets/100069489/11e0741f-f027-4c0b-8924-c9d41af1b5c8)

Here we go, now its redirected to NVirginia region and data too updated

![image](https://github.com/kohlidevops/aws-disaster-recovery-with-Warm-Standby/assets/100069489/02f24a94-9e15-4187-a2aa-750e3272cd26)

That's it.







