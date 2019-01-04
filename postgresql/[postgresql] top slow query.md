## How to find top slow query in postgresql

* Install pg_stat_statement

* Configuare pg_stat_statement

* Analyze top sql

  * top io sql

    ```sql
    select userid::regrole, dbid, query from pg_stat_statements order by (blk_read_time+blk_write_time)/calls desc limit 5;
    ```

  * Top time sql

    ```sql
    select userid::regrole, dbid, query from pg_stat_statements order by mean_time desc limit 5; 
    ```

  * Top memory sql

    ```sql
    select userid::regrole, dbid, query from pg_stat_statements order by (shared_blks_hit+shared_blks_dirtied) desc limit 5;   
    ```

  * Top temporary space sql

    ```sql
    select userid::regrole, dbid, query from pg_stat_statements order by temp_blks_written desc limit 5;
    ```

* More Info on top query

  ```sql
  set client_min_messages=debug5;
  set log_checkpoints = on;
  set log_error_verbosity = verbose ;
  set log_lock_waits = on;                  
  set log_replication_commands = off;
  set log_temp_files = 0;
  set track_activities = on;
  set track_counts = on;
  set track_io_timing = on;
  set track_functions = 'all';
  set trace_sort=on;
  set log_statement_stats = off;
  set log_parser_stats = on;
  set log_planner_stats = on;
  set log_executor_stats = on;
  set log_autovacuum_min_duration=0;
  set deadlock_timeout = '1s';
  set debug_print_parse = off;
  set debug_print_rewritten = off;
  set debug_print_plan = off;
  set debug_pretty_print = on;
   
  
  explain (analyze,verbose,timing,costs,buffers) select count(*),relkind from pg_class group by relkind order by count(*) desc limit 1;
  ```
