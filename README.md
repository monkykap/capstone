# capstone


working configs in config/working

some commands

bro (this config has frequency and domain creaton dates, and subdomain,parent domain ect.) -> PORT 5555

 /opt/logstash/bin/logstash -v -f ~/capstone/config_working/bro_dns.conf 
 
 send over tcp with nc
 
  nc 127.0.0.1 5555 < /labs/bootcamp/day2/dns.log
  
  
  json (may want to change index name or tag) -> PORT 5556
  
   /opt/logstash/bin/logstash -v -f ~/capstone/config_working/json.conf 
   
    nc 127.0.0.1 5556 < /labs/bootcamp/day4/windows.log
    
    
   SAMPLE BASE CONFIGS

BRO

tcp {
    port => 5555
  }
}


filter {
  csv {
  	columns => ["timestamp","uid","source_ip","source_port","destination_ip","destination_port","protocol","transaction_id","rtt","query","query_class","query_class_name","query_type","query_type_name","rcode","rcode_name","aa","tc","rd","ra","z","answers","ttls","rejected"]

 	separator => "	"
  }

    date {
      match => ["timestamp", "UNIX"]
      remove_field => [ "EventTime"]
    }
  

    mutate {
		add_tag => [ "bro"]
	}
}

output {
  elasticsearch {
    index => "bro-test"
  }
}


JSON


input {
  tcp {
    port => 5556
  }
}
filter {
  json {
    source => "message"
  }
  mutate { 
    remove_field => "Message"
  }

}

output {
  elasticsearch {
    	index => "lab-json"
  }

}
  
  
  
