# bind
```
sys_bind
  -> inet_bind
    -> inet_csk_get_port
      -> goto have_snum:
      -> goto tb_not_found:  tb == NULL
        -> inet_bind_bucket_create
      -> goto success:
        -> inet_bind_hash
  -> fput_light
```

# listen
```
sys_listen
  -> inet_listen
    -> inet_csk_listen_start
      -> reqsk_queue_alloc
      -> inet_csk_delack_init
      -> set sk->sk_state = TCP_LISTEN
      -> inet_csk_get_port
        -> goto have_snum:
        -> goto tb_found:
          -> inet_csk_bind_conflict
        -> goto tb_not_found:  tb != NULL
      -> inet_hash
        -> __inet_hash  // tcp_hashinfo.listening_hash[X] add node
```

# connect

# tcp_v4_rcv

# read

# write
