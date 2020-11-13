# Monerod Restricted RPC Mode
Neptune Research

**Applies to**: Monero 0.12.3.0  
**Reference**: ```rpc/core_rpc_server.cpp```

The following changes occur when Monerod RPC runs in Restricted Mode via ```restricted-rpc``` and/or ```rpc-restricted-bind-port``` options.

### RPC methods not registered

- flush_txpool
- generateblocks
- get_alternate_chains
- get_bans
- get_coinbase_tx_sum
- get_peer_list
- in_peers
- mining_status
- on_get_connections
- on_set_bans
- out_peers
- relay_tx
- save_bc
- set_bans
- set_limit
- set_log_categories
- set_log_hash_rate
- set_log_level
- start_mining
- start_save_graph
- stop_daemon
- stop_mining
- stop_save_graph
- sync_info
- update

### RPC methods which have parameter ```include_sensitive_data```

```include_sensitive_data``` is a parameter to the following functions.  
  
Callers provide this parameter as ```include_sensitive_data = !restricted```; so in Restricted Mode, ```include_sensitive_data = FALSE```, and when not in Restricted Mode, ```include_sensitive_data = TRUE```.  

- get_pool_transactions_and_spent_keys_info
- get_pool_transaction_hashes
- get_pool_transaction_stats

### Special rules

| RPC method | Restricted mode rule |
| - | - |
| get_info | free_space is set to `MAX_INT` instead of real free space value |
| get_random_outs | limits size of request 'amounts' (`< 100`) and 'outs_count' (`< MAX_RESTRICTED_FAKE_OUTS_COUNT`) |
| get_outs | limits size of request 'outputs' (`< MAX_RESTRICTED_GLOBAL_FAKE_OUTS_COUNT`) |