## MultiAZ:

1. Allows you to have an exact copy of your production database in another AZ.
2. AWS handles the replication for you.
3. In event of planned DB maitainence, DB instance failure or AZ failure, RDS will automatically failover to the standby so that DB operations can resume quickly without admin intervention.
4. MultiAZ is for disaster recovery only
5. It is available for :
 - SQL server
 - Oracle
 - MySQL server
 - PostGre SQL
 - MariaDB
6. Can force failover from one AZ to another by rebooting RDS instance.


## Read Replicas:

1. Allows to have read only copt of Prod DB.
2. Achieved by using Asynchronous replication from primary RDS instance to the read replica. Can use read replica for very read-heavy database workloads.
3. Read replicas are available for following DB:
 - MySQL server
 - Postgre SQL
 - MariaDB
 - Oracle
 - Aurora
4. Used for scaling not for disaster recovery
5. Must have automatic backups turned on in order to deploy read replicas.
6. 5 read replicas copies of any database.
7. Can have read replicas of read replicas.
8. Each read replica will have its own DNS endpoint.
9. Can have read replicas taht have multi az
10. Can create read replicas of multi-az source dbs.
11. Read replicas can be promoted to be its own DB.
12. Can  have read replicas in second region.

## Tips for read replicas:

1. Can be multi-az
2. used to increase performance.
3. must have backups turned on
4. can be in different regions
5. can be aurora of mysql
6. can be promoted to master, this will break read replica multi-az

