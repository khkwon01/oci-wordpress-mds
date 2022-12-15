# oci-wordpress-mds

Deploy Wordpress on Oracle Cloud Intrastructure (OCI) and MySQL Database Service (MDS) using these Terraform modules.

The same modules are used as Resource Manager Stack.

The latest stack can be downloaded directly in the releases (the zip file)

[![Deploy to Oracle Cloud](https://oci-resourcemanager-plugin.plugins.oci.oraclecloud.com/latest/deploy-to-oracle-cloud.svg)](https://cloud.oracle.com/resourcemanager/stacks/create?zipUrl=https://github.com/lefred/oci-wordpress-mds/releases/download/v1.9.1/stack_wordpress_mds.zip)


<hr/>

# It need to install LudicrousDB for testing read replica for MySQL.

1. login wordpress server firstly.
2. install LudicrousDB plgin to Word press application.
```
  [opc@wordpressserver1 ~]$ cd /var/www/html/wp-content/plugins
  [opc@wordpressserver1 ~]$ sudo wget https://github.com/stuttter/ludicrousdb/archive/refs/heads/master.zip
  [opc@wordpressserver1 plugins]$ sudo unzip master.zip
  [opc@wordpressserver1 plugins]$ sudo mv ludicrousdb-master ludicrousdb
  [opc@wordpressserver1 plugins]$ sudo rm master.zip
  [opc@wordpressserver1 plugins]$ sudo chown -R apache. ludicrousdb
  [opc@wordpressserver1 plugins]$ sudo cp ludicrousdb/ludicrousdb/drop-ins/db.php ../db.php
  [opc@wordpressserver1 plugins]$ sudo cp ludicrousdb/ludicrousdb/drop-ins/db-config.php ../../
```
3. Modify the information of db config like the following example.
```
  [opc@wordpressserver1 ~]$ cd /var/www/html/
  [opc@wordpressserver1 html]$ sudo vim db-config.php
  
  $wpdb->add_database( array(
  'host'     => "10.0.1.152", <-- edit_me     // If port is other than 3306, use host:port.
  'user'     => DB_USER,
  'password' => DB_PASSWORD,
  'name'     => DB_NAME,
  'write'    => 0,
  'read'     => 1,            <-- edit_me
  'dataset'  => 'global',
  'timeout'  => 0.2,     
   ) );
```
4. Write test php pages 
```
  <!--?php
    $result = $wpdb--->get_results("select @@hostname as host");
    echo "<strong>host:</strong> " . $result[0]->host;
  ?>
```
