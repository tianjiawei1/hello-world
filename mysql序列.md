**set global log_bin_trust_function_creators=TRUE;**<br/> 
create function currval(v_seq_name VARCHAR(50))  returns integer(11) <br/> 
begin<br/> 
 declare value integer; <br/> 
 set value = 0; <br/> 
 select current_val into value  from sequence where seq_name = v_seq_name; <br/> 
   return value;<br/> 
end;<br/> 
