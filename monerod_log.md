# Monerod Log
Neptune Research

**Written for**: Monero 0.12.3.0  
**Some updates regarding**: Monero 0.16.0.1  

## Table of Contents
- [Category List from Daemon RPC Documentation](#category-list-from-daemon-rpc-documentation)
- [Log Categories per C++ Class](#log-categories-per-c-class)
- [Log Level](#log-level)
- [Daemon Log Level](#daemon-log-level)

## Category List from Daemon RPC Documentation

Source: [Monero Project Daemon RPC documentation: set_log_categories](https://getmonero.org/resources/developer-guides/daemon-rpc.html#set_log_categories)

- account
- bcutil
- blockchain
- blockchain.db
- blockchain.db.lmdb
- bulletproofs
- checkpoints
- cn
- cn.block_queue
- daemon
- daemon.rpc
- debugtools.deserialize
- debugtools.objectsizes
- default  
- device.ledger
- difficulty
- hardfork
- i18n
- logging  
- miner
- multisig
- net  
- net.cn
- net.dl
- net.dns
- net.http  
- net.p2p  
- net.throttle  
- perf
- ringct
- stacktrace
- tests.core
- txpool
- updates
- WalletAPI
- wallet.gen_multisig
- wallet.ringdb
- wallet.rpc
- wallet.simplewallet
- wallet.wallet2
- ```*``` for all facilities  
- **Empty** to disable logging

## Log Categories per C++ Class

Every Monero source file defines the log category for its class at the top. The following list of Log Categories may be generated via this ```grep``` command:

    grep -r "define MONERO_DEFAULT_LOG_CATEGORY" monero/src/* > log_categories.txt

Monero Class | Log Category
------- | -------
blockchain_db/lmdb/db_lmdb.cpp | blockchain.db.lmdb
blockchain_db/blockchain_db.cpp | blockchain.db
blockchain_utilities/blockchain_export.cpp | bcutil
blockchain_utilities/blockchain_blackball.cpp | bcutil
blockchain_utilities/blockchain_import.cpp | bcutil
blockchain_utilities/bootstrap_file.cpp | bcutil
blockchain_utilities/blocksdat_file.cpp | bcutil
blockchain_utilities/blockchain_usage.cpp | bcutil
checkpoints/checkpoints.cpp | checkpoints
common/perf_timer.h | perf
common/stack_trace.cpp | stacktrace
common/perf_timer.cpp | perf
common/i18n.cpp | i18n
common/updates.cpp | updates
common/download.cpp | net.dl
common/dns_utils.cpp | net.dns
cryptonote_basic/cryptonote_basic_impl.cpp | cn
cryptonote_basic/miner.cpp | miner
cryptonote_basic/cryptonote_format_utils.cpp | cn
cryptonote_basic/difficulty.cpp | difficulty
cryptonote_basic/account.cpp | account
cryptonote_basic/hardfork.cpp | hardfork
cryptonote_core/blockchain.cpp | blockchain
cryptonote_core/cryptonote_core.cpp | cn
cryptonote_core/tx_pool.cpp | txpool
cryptonote_protocol/block_queue.cpp | cn.block_queue
cryptonote_protocol/cryptonote_protocol_handler.inl | net.cn
cryptonote_protocol/cryptonote_protocol_handler-base.cpp | net.cn
cryptonote_protocol/block_queue.h | cn.block_queue
daemon/daemon.h | daemon
daemon/command_server.cpp | daemon
daemon/p2p.h | daemon
daemon/command_parser_executor.cpp | daemon
daemon/rpc.h | daemon
daemon/rpc_command_executor.h | daemon
daemon/executor.cpp | daemon
daemon/rpc_command_executor.cpp | daemon
daemon/executor.h | daemon
daemon/main.cpp | daemon
daemon/daemon.cpp | daemon
daemon/protocol.h | daemon
daemon/core.h | daemon
debug_utilities/object_sizes.cpp | debugtools.objectsizes
debug_utilities/cn_deserialize.cpp | debugtools.deserialize
device/device_ledger.cpp | device.ledger
device/log.cpp | device.ledger
gen_multisig/gen_multisig.cpp | wallet.gen_multisig
mnemonics/electrum-words.cpp | mnemonic
multisig/multisig.cpp | multisig
p2p/net_node.inl | net.p2p
ringct/rctTypes.cpp | ringct
ringct/rctOps.cpp | ringct
ringct/rctSigs.cpp | ringct
ringct/bulletproofs.cc | bulletproofs
rpc/core_rpc_server.cpp | daemon.rpc
simplewallet/simplewallet.cpp | wallet.simplewallet
simplewallet/simplewallet.h | wallet.simplewallet
wallet/wallet_rpc_server.cpp | wallet.rpc
wallet/wallet2.cpp | wallet.wallet2
wallet/wallet2.h | wallet.wallet2
wallet/api/wallet.cpp | WalletAPI
wallet/api/wallet_manager.cpp | WalletAPI
wallet/wallet_rpc_server_commands_defs.h | wallet.rpc
wallet/wallet_args.cpp | wallet.wallet2
wallet/wallet_rpc_server.h | wallet.rpc
wallet/ringdb.cpp | wallet.ringdb

## Log Level (current as of Monero 0.16.0.1)
According to [monero-project/monero@5833d66](https://github.com/monero-project/monero/commit/5833d66f6540e7b34e10ddef37c2b67bd501994b), these are the Log Levels.

    FATAL, ERROR, WARNING, INFO, DEBUG, TRACE

## Daemon Log Level (updated for Monero 0.16.0.1)
These are preset combinations of Log Categories and Log Levels.  
Source: [contrib/epee/src/mlog.cpp: get_default_categories()](https://github.com/monero-project/monero/blob/master/contrib/epee/src/mlog.cpp#L94)

Daemon Log Level | Result
------------ | -------------
0 | ```*:WARNING,net:FATAL,net.http:FATAL,net.ssl:FATAL,net.p2p:FATAL,net.cn:FATAL,global:INFO,verify:FATAL,serialization:FATAL,daemon.rpc.payment:ERROR,stacktrace:INFO,logging:INFO,msgwriter:INFO```
1 | ```*:INFO,global:INFO,stacktrace:INFO,logging:INFO,msgwriter:INFO,perf.*:DEBUG```
2 | ```*:DEBUG```
3 | ```*:TRACE,*.dump:DEBUG```
4 | ```*:TRACE```