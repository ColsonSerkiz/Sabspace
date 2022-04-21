Создаем сервисные файлы (вставить в консоль каждый блок целиком ничего не меняя)
printf "[Unit]
Description=Subspace Node
After=network.target
[Service]
Type=simple
User=subspace
ExecStart=$(which subspace-node) \\
            --chain testnet \\
            --wasm-execution compiled \\
            --execution wasm --bootnodes \"/dns/farm-rpc.subspace.network/tcp/30333/p2p/12D3KooWPjMZuSYj35ehced2MTJFf95upwpHKgKUrFRfHwohzJXr\" \\
            --rpc-cors all \\
            --rpc-methods unsafe \\
            --ws-external \\
            --validator \\
            --telemetry-url \"wss://telemetry.polkadot.io/submit/ 1\" \\
            --telemetry-url \"wss://telemetry.subspace.network/submit 1\" \\
            --name $SUBSPACE_NODE_NAME
Restart=always
RestartSec=10
LimitNOFILE=10000
[Install]
WantedBy=multi-user.target" > /etc/systemd/system/subspace-node.service


printf "[Unit]
Description=Subspace Farmer
Requires=subspace-node.service
After=network.target
After=subspace-node.service
[Service]
Type=simple
User=subspace
ExecStart=$(which subspace-farmer) farm --reward-address $SUBSPACE_WALLET_ADDRESS
Restart=always
RestartSec=10
LimitNOFILE=10000
[Install]
WantedBy=multi-user.target" > /etc/systemd/system/subspace-farmer.service
