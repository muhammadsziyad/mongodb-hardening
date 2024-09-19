
# MongoDB Server Hardening 

## Overview

This guide provides step-by-step instructions for each method to enhance the security of your MongoDB 
installation.

## Hardening Techniques

### 1. Update MongoDB Regularly
- **Step 1:** Check for the latest MongoDB version from the [official MongoDB 
website](https://www.mongodb.com/try/download/community).
- **Step 2:** Follow the installation instructions for your operating system to upgrade MongoDB.

### 2. Use Strong Authentication
- **Step 1:** Enable authentication by setting `security.authorization` to `enabled` in `mongod.conf`:
  ```yaml
  security:
    authorization: "enabled"
    ```

-   **Step 2:** Create a strong administrator user:
    
    ```
    mongo
    use admin
    db.createUser({
      user: "admin",
      pwd: "StrongAdminPassword!",
      roles: [{ role: "userAdminAnyDatabase", db: "admin" }]
    })
    ``` 
    

### 3. Remove Unused User Accounts

-   **Step 1:** Log in to MongoDB:
    
    
    ```
    mongo -u admin -p StrongAdminPassword! --authenticationDatabase admin
    ``` 
    
-   **Step 2:** List all users:
    
    
    ```
    use admin
    db.getUsers()
    ``` 
    
-   **Step 3:** Drop unused users:
    
    ```
    db.dropUser("username")
    ``` 
    

### 4. Restrict Remote Access

-   **Step 1:** Edit `mongod.conf` to bind MongoDB to localhost only:
    
    
    ```
    net:
      bindIp: 127.0.0.1
      ``` 
    
-   **Step 2:** Restart MongoDB:
    
    
    ```
    sudo systemctl restart mongod
    ``` 
    

### 5. Enable TLS/SSL Encryption

-   **Step 1:** Generate TLS/SSL certificates.
-   **Step 2:** Configure MongoDB to use TLS/SSL in `mongod.conf`:
    
    
    ```
    net:
      tls:
        mode: requireTLS
        certificateKeyFile: /path/to/your/certificate.pem
        ``` 
    
-   **Step 3:** Restart MongoDB:
    
    
    ```
    sudo systemctl restart mongod
    ``` 
    

### 6. Use Role-Based Access Control (RBAC)

-   **Step 1:** Define roles and assign appropriate privileges:
   
    
    ```
    use admin
    db.createRole({
      role: "readWriteRole",
      privileges: [
        { resource: { db: "yourDB", collection: "" }, actions: [ "find", "insert", "update", "remove" ] }
      ],
      roles: []
    })
    ``` 
    
-   **Step 2:** Assign roles to users:
    
    
    ```
    db.grantRolesToUser("username", [{ role: "readWriteRole", db: "yourDB" }])
    ``` 
    

### 7. Secure MongoDB Configuration Files

-   **Step 1:** Set proper file permissions for configuration files:
    
    
    ```
    sudo chmod 600 /etc/mongod.conf
    sudo chown mongod:mongod /etc/mongod.conf
    ``` 
    

### 8. Enable MongoDB Auditing

-   **Step 1:** Configure auditing in `mongod.conf`:
    
    
    ```
    systemLog:
      destination: file
      path: /var/log/mongodb/audit.log
      logAppend: true
    auditLog:
      destination: file
      path: /var/log/mongodb/audit.log
      ``` 
    
-   **Step 2:** Restart MongoDB:
    
    
    ```
    sudo systemctl restart mongod
    ``` 
    

### 9. Limit Resource Usage

-   **Step 1:** Set resource limits in `mongod.conf`:
    
    
    ```
    net:
      maxIncomingConnections: 1000
      ``` 
    

### 10. Enable Access Control

-   **Step 1:** Ensure `authorization` is enabled in `mongod.conf`:
    
    
    ```
    security:
      authorization: "enabled"
      ``` 
    
-   **Step 2:** Restart MongoDB:
    
    
    ```sudo systemctl restart mongod``` 
    

### 11. Use IP Whitelisting

-   **Step 1:** Configure IP whitelisting in `mongod.conf`:
    
    
    ```
    net:
      bindIp: 127.0.0.1,192.168.1.100
      ``` 
    
-   **Step 2:** Restart MongoDB:
    
    
    `sudo systemctl restart mongod` 
    

### 12. Encrypt Data at Rest

-   **Step 1:** Use MongoDB’s built-in encryption-at-rest features or third-party tools.

### 13. Secure MongoDB Data Directory

-   **Step 1:** Set proper permissions:
    
    
    ```
    sudo chmod 700 /var/lib/mongodb
    sudo chown mongod:mongod /var/lib/mongodb
    ``` 
    

### 14. Backup Regularly

-   **Step 1:** Use `mongodump` to create backups:
    
    
    ```
    mongodump --out /backup/dump
    ``` 
    
-   **Step 2:** Schedule regular backups with `cron` or a similar scheduler.

### 15. Use MongoDB Encryption Keys

-   **Step 1:** Configure encryption keys in `mongod.conf`:
    
    
    ```
    security:
      keyFile: /path/to/your/keyfile
      ``` 
    
-   **Step 2:** Restart MongoDB:
    
    
    `sudo systemctl restart mongod` 
    

### 16. Disable Unnecessary MongoDB Modules

-   **Step 1:** Review and disable unnecessary modules or features.

### 17. Monitor MongoDB Logs

-   **Step 1:** Regularly review logs located at `/var/log/mongodb/mongod.log`.

### 18. Limit Maximum Connections

-   **Step 1:** Set maximum connections in `mongod.conf`:
   
    
    ```
    net:
      maxIncomingConnections: 1000
      ``` 
    

### 19. Use `mongod` in a Private Network

-   **Step 1:** Place MongoDB server in a private subnet to limit exposure.

### 20. Avoid Using Default Ports

-   **Step 1:** Change MongoDB default port in `mongod.conf`:
   
    
    ```
    net:
      port: 27018
      ``` 
    
-   **Step 2:** Restart MongoDB:
    
    
    `sudo systemctl restart mongod` 
    

### 21. Use `mongorestore` Securely

-   **Step 1:** Ensure that `mongorestore` uses secure credentials and connections.

### 22. Enable Disk Encryption

-   **Step 1:** Use operating system or hardware-based disk encryption to secure data.

### 23. Regularly Update MongoDB Software

-   **Step 1:** Keep MongoDB and its dependencies up-to-date with the latest patches and updates.

### 24. Secure MongoDB with Host-Based Firewall

-   **Step 1:** Configure firewall rules to restrict access to MongoDB.

### 25. Avoid Using `root` User for Application Access

-   **Step 1:** Create specific users for applications and avoid using the `root` user.

### 26. Implement Network Security Groups (NSGs)

-   **Step 1:** Configure NSGs to restrict traffic to the MongoDB server.

### 27. Limit `maxWriteBatchSize`

-   **Step 1:** Configure `maxWriteBatchSize` in `mongod.conf`:
    
    ```
    operationProfiling:
      slowOpThresholdMs: 100
      ``` 
    

### 28. Review and Update Security Policies

-   **Step 1:** Regularly review and update security policies and configurations.

### 29. Disable Unused MongoDB Services

-   **Step 1:** Stop and disable any unused MongoDB services or daemons.

### 30. Implement Rate Limiting

-   **Step 1:** Use tools or middleware to implement rate limiting for MongoDB access.

### 31. Configure `mongod` Authentication

-   **Step 1:** Ensure `mongod` uses authentication by configuring `mongod.conf`:
    
    
    ```
    security:
      authorization: "enabled"
      ``` 
    

### 32. Use Read-Only Users

-   **Step 1:** Create read-only users for applications that do not require write access:
    
    
    ```
    db.createUser({
      user: "readonlyuser",
      pwd: "ReadonlyPassword!",
      roles: [{ role: "read", db: "yourDB" }]
    })
    ``` 
    

### 33. Enable Query Profiling

-   **Step 1:** Enable query profiling in `mongod.conf`:
    
    
    ```
    operationProfiling:
      slowOpThresholdMs: 100
      ``` 
    

### 34. Secure Backup Files

-   **Step 1:** Encrypt and secure backup files.

### 35. Limit `maxWriteBatchSize`

-   **Step 1:** Configure `maxWriteBatchSize` to prevent large write operations from affecting 
performance.

### 36. Disable Remote Shell Access

-   **Step 1:** Ensure remote shell access is disabled unless needed.

### 37. Monitor and Patch Vulnerabilities

-   **Step 1:** Regularly check for MongoDB vulnerabilities and apply patches.

### 38. Use Connection Pooling

-   **Step 1:** Implement connection pooling to manage database connections efficiently.

### 39. Avoid Using Default Configuration

-   **Step 1:** Customize MongoDB configuration files instead of using default settings.

### 40. Use Strong Encryption Standards

-   **Step 1:** Ensure encryption methods meet current security standards.

### 41. Secure Admin Interfaces

-   **Step 1:** Restrict access to MongoDB admin interfaces.

### 42. Implement API Security

-   **Step 1:** Secure MongoDB APIs with proper authentication and authorization.

### 43. Regularly Test Security Configurations

-   **Step 1:** Perform regular security testing and audits.

### 44. Enable IP Whitelisting

-   **Step 1:** Configure IP whitelisting in `mongod.conf` to allow connections only from specific IPs.

### 45. Use `mongos` for Sharding

-   **Step 1:** Secure sharded MongoDB clusters by configuring `mongos` with appropriate access controls.

### 46. Avoid Storing Sensitive Data in Plaintext

-   **Step 1:** Use encryption or hashing for sensitive data stored in MongoDB.

### 47. Implement Detailed Logging

-   **Step 1:** Configure MongoDB logging to include detailed information for monitoring.

### 48. Use Secure Communication Channels

-   **Step 1:** Ensure all communication with MongoDB is encrypted using TLS/SSL.

### 49. Restrict Access to Backup Files

-   **Step 1:** Secure and limit access to MongoDB backup files.

### 50. Perform Regular Security Reviews

-   **Step 1:** Conduct regular security reviews to ensure that MongoDB configurations are up-to-date and 
secure.
