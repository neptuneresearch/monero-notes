# Monerod Log
Neptune Research

**Version**: Monero 0.17.1.3  

## Table of Contents
- [Category List from Daemon RPC Documentation](#category-list-from-daemon-rpc-documentation)
- [Log Categories per C++ Class](#log-categories-per-c-class)
- [Log Level](#log-level)
- [Daemon Log Level](#daemon-log-level)

## Category List from Daemon RPC Documentation

Source: [Monero Project Daemon RPC documentation: set_log_categories](https://getmonero.org/resources/developer-guides/daemon-rpc.html#set_log_categories)

- `*` - All facilities
- default
- net
- net.http
- net.p2p
- logging
- net.throttle
- blockchain.db
- blockchain.db.lmdb
- bcutil
- checkpoints
- net.dns
- net.dl
- i18n
- perf
- stacktrace
- updates
- account
- cn
- difficulty
- hardfork
- miner
- blockchain
- txpool
- cn.block_queue
- net.cn
- daemon
- debugtools.deserialize
- debugtools.objectsizes
- device.ledger
- wallet.gen_multisig
- multisig
- bulletproofs
- ringct
- daemon.rpc
- wallet.simplewallet
- WalletAPI
- wallet.ringdb
- wallet.wallet2
- wallet.rpc
- tests.core

## Log Categories per C++ Class

Every Monero source file defines the log category for its class at the top. The following list of Log Categories may be generated via this ```grep``` command:

    grep -r "define MONERO_DEFAULT_LOG_CATEGORY" monero/src/* > log_categories.txt

Monero Class | Log Category
------- | -------
blockchain_db/blockchain_db.cpp | blockchain.db
blockchain_db/lmdb/db_lmdb.cpp | blockchain.db.lmdb
blockchain_utilities/blockchain_export.cpp | bcutil
blockchain_utilities/blockchain_stats.cpp | bcutil
blockchain_utilities/blocksdat_file.cpp | bcutil
blockchain_utilities/blockchain_blackball.cpp | bcutil
blockchain_utilities/blockchain_prune.cpp | bcutil
blockchain_utilities/blockchain_import.cpp | bcutil
blockchain_utilities/blockchain_usage.cpp | bcutil
blockchain_utilities/blockchain_depth.cpp | bcutil
blockchain_utilities/bootstrap_file.cpp | bcutil
blockchain_utilities/blockchain_prune_known_spent_data.cpp | bcutil
blockchain_utilities/blockchain_ancestry.cpp | bcutil
checkpoints/checkpoints.cpp | checkpoints
common/updates.cpp | updates
common/download.cpp | net.dl
common/dns_utils.cpp | net.dns
common/util.cpp | util
common/spawn.cpp | spawn
common/perf_timer.cpp | perf
common/i18n.cpp | i18n
common/notify.cpp | notify
common/stack_trace.cpp | stacktrace
cryptonote_basic/account.cpp | account
cryptonote_basic/miner.cpp | miner
cryptonote_basic/cryptonote_basic_impl.cpp | cn
cryptonote_basic/hardfork.cpp | hardfork
cryptonote_basic/difficulty.cpp | difficulty
cryptonote_basic/cryptonote_format_utils.cpp | cn
cryptonote_core/cryptonote_core.cpp | cn
cryptonote_core/tx_pool.cpp | txpool
cryptonote_core/blockchain.cpp | blockchain
cryptonote_core/tx_sanity_check.cpp | verify
cryptonote_protocol/cryptonote_protocol_handler.inl | net.cn
cryptonote_protocol/block_queue.h | cn.block_queue
cryptonote_protocol/levin_notify.cpp | net.p2p.tx
cryptonote_protocol/cryptonote_protocol_handler-base.cpp | net.cn
cryptonote_protocol/block_queue.cpp | cn.block_queue
daemon/protocol.h | daemon
daemon/rpc.h | daemon
daemon/main.cpp | daemon
daemon/core.h | daemon
daemon/executor.h | daemon
daemon/rpc_command_executor.cpp | daemon
daemon/daemon.cpp | daemon
daemon/p2p.h | daemon
daemon/daemon.h | daemon
daemon/command_server.cpp | daemon
daemon/executor.cpp | daemon
daemon/command_parser_executor.cpp | daemon
daemon/rpc_command_executor.h | daemon
debug_utilities/dns_checks.cpp | debugtools.dnschecks
debug_utilities/object_sizes.cpp | debugtools.objectsizes
debug_utilities/cn_deserialize.cpp | debugtools.deserialize
device/device_io_hid.cpp | device.io
device/device_ledger.cpp | device.ledger
device/log.cpp | device
device/log.cpp | device.ledger
device_trezor/device_trezor_base.cpp | device.trezor
device_trezor/device_trezor.cpp | device.trezor
device_trezor/trezor/transport.cpp | device.trezor.transport
gen_multisig/gen_multisig.cpp | wallet.gen_multisig
gen_ssl_cert/gen_ssl_cert.cpp | gen_ssl_cert
hardforks/hardforks.cpp | blockchain.hardforks
mnemonics/electrum-words.cpp | mnemonic
multisig/multisig.cpp | multisig
p2p/net_node.inl | net.p2p
ringct/rctTypes.cpp | ringct
ringct/rctSigs.cpp | ringct
ringct/rctOps.cpp | ringct
ringct/multiexp.cc | multiexp
ringct/bulletproofs.cc | bulletproofs
rpc/zmq_pub.cpp | net.zmq
rpc/rpc_payment.cpp | daemon.rpc.payment
rpc/rpc_payment_signature.cpp | daemon.rpc.payment
rpc/zmq_server.cpp | net.zmq
rpc/core_rpc_server.h | daemon.rpc
rpc/core_rpc_server.cpp | daemon.rpc
rpc/bootstrap_daemon.cpp | daemon.rpc.bootstrap_daemon
simplewallet/simplewallet.cpp | wallet.simplewallet
simplewallet/simplewallet.h | wallet.simplewallet
wallet/wallet_rpc_payments.cpp | wallet.wallet2.rpc_payments
wallet/message_store.h | wallet.mms
wallet/api/wallet_manager.cpp | WalletAPI
wallet/api/wallet.cpp | WalletAPI
wallet/wallet_args.cpp | wallet.wallet2
wallet/ringdb.cpp | wallet.ringdb
wallet/message_store.cpp | wallet.mms
wallet/message_transporter.cpp | wallet.mms
wallet/wallet2.cpp | wallet.wallet2
wallet/wallet2.h | wallet.wallet2
wallet/wallet_rpc_server_commands_defs.h | wallet.rpc
wallet/wallet_rpc_server.h | wallet.rpc
wallet/wallet_rpc_server.cpp | wallet.rpc

## Log Level
According to [monero-project/monero@5833d66](https://github.com/monero-project/monero/commit/5833d66f6540e7b34e10ddef37c2b67bd501994b), these are the Log Levels.

    FATAL, ERROR, WARNING, INFO, DEBUG, TRACE

## Daemon Log Level
These are preset combinations of Log Categories and Log Levels.  
Source: [contrib/epee/src/mlog.cpp: get_default_categories()](https://github.com/monero-project/monero/blob/master/contrib/epee/src/mlog.cpp#L94)

Daemon Log Level | Result
------------ | -------------
0 | ```*:WARNING,net:FATAL,net.http:FATAL,net.ssl:FATAL,net.p2p:FATAL,net.cn:FATAL,daemon.rpc:FATAL,global:INFO,verify:FATAL,serialization:FATAL,daemon.rpc.payment:ERROR,stacktrace:INFO,logging:INFO,msgwriter:INFO```
1 | ```*:INFO,global:INFO,stacktrace:INFO,logging:INFO,msgwriter:INFO,perf.*:DEBUG```
2 | ```*:DEBUG```
3 | ```*:TRACE,*.dump:DEBUG```
4 | ```*:TRACE```
