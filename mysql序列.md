**set global log_bin_trust_function_creators=TRUE;**
create function currval(v_seq_name VARCHAR(50))   
returns integer(11) 
begin
 declare value integer;
 set value = 0;
 select current_val into value  from sequence where seq_name = v_seq_name;
   return value;
end;
