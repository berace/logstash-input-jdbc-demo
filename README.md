# logstash-input-jdbc-demo
1. 安装插件
logstash-plugin install logstash-input-jdbc
2. 新建配置文件
如jdbc.conf

input {
    jdbc {
      jdbc_connection_string => "jdbc:mysql://127.0.0.1:3306/invoice"
      jdbc_user => "root"
      jdbc_password => "1234"
      jdbc_driver_library => "mysql/mysql-connector-java-5.1.39.jar"
      jdbc_driver_class => "com.mysql.jdbc.Driver"
      jdbc_paging_enabled => "true"
      jdbc_page_size => "50000"

	codec => plain { charset => "UTF-8"}

	use_column_value => true
	tracking_column => id      
	record_last_run => true     
	last_run_metadata_path => "mysql/station_parameter.txt"

	#jdbc_default_timezone => "Asia/Shanghai"
 
	statement_filepath => "mysql/jdbc.sql"
      

	clean_run => false

	schedule => "* * * * *"
	type => "jdbc"
    }
}


output {
    elasticsearch {
        hosts => "127.0.0.1:9200"
        index => "db_anytest"
        document_type => "table_anytest"
        document_id => "%{id}"
        
    }

    stdout {
        codec => json_lines
    }
    
}
