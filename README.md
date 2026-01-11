
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ESHART ZOO | V28 Cyber-Zoo</title>
    
    <script src="https://bundle.run/buffer@6.0.3"></script>
    <script src="https://unpkg.com/@solana/web3.js@1.95.3/lib/index.iife.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@solana/spl-token@0.3.9/lib/index.iife.min.js"></script>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@900&family=Inter:wght@400;700&display=swap" rel="stylesheet">
    
    <style>
        :root { --accent: #14F195; --bg: #010204; }
        body { 
            background-color: var(--bg); 
            color: white; 
            font-family: 'Inter', sans-serif; 
            margin: 0;
            /* BACKGROUND IMAGE SETUP */
            background-image: linear-gradient(rgba(1, 2, 4, 0.85), rgba(1, 2, 4, 0.85)), url('https://images.unsplash.com/photo-1614850523296-d8c1af93d400?q=80&w=2070&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }
        .font-heading { font-family: 'Orbitron', sans-serif; }
        .glass { background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.08); backdrop-filter: blur(15px); }
        .shart-gradient { background: linear-gradient(90deg, #9945FF, #14F195); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .card-Common { border-left: 3px solid #475569; }
        .card-Rare { border-left: 3px solid #3b82f6; }
        .card-Legendary { border-left: 3px solid #ff00ff; }
        .card-Mythic { border-left: 3px solid #fbbf24; border-right: 3px solid #fbbf24; box-shadow: 0 0 30px #fbbf2444; }
        
        .tg-btn { transition: all 0.3s ease; border: 1px solid rgba(0, 136, 204, 0.3); }
        .tg-btn:hover { background: #0088cc; transform: translateY(-2px); }
    </style>
</head>
<body class="min-h-screen pb-20">

    <nav class="fixed top-0 w-full z-[100] glass border-b border-white/5 px-6 py-4 flex justify-between items-center">
        <div class="flex items-center gap-3">
            <img src="https://i.ibb.co/0jdXM3fZ/1767978719058-2.jpg" class="h-10 w-10 rounded-lg shadow-lg shadow-[#14F195]/20">
            <span class="text-xl font-black font-heading shart-gradient uppercase italic">ESHART ZOO</span>
        </div>
        
        <div class="flex items-center gap-4">
            <a href="https://t.me/ElonSHARTS" target="_blank" class="tg-btn hidden md:flex items-center gap-2 bg-black/40 px-4 py-2 rounded-xl text-[10px] font-black uppercase">
                <svg width="14" height="14" viewBox="0 0 24 24" fill="white"><path d="M12 0c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm4.462 15.998l-.017.012-3.469 1.912c-.272.149-.599.045-.747-.227l-1.012-1.874-1.712.851-.001.001-.001.001c-.662.333-1.42-.147-1.42-.888v-4.52l-2.062-1.031c-.272-.137-.382-.464-.245-.736s.464-.382.736-.245l11.161 5.58c.272.137.382.464.245.736s-.463.382-.736.241z"/></svg>
                Telegram
            </a>
            
            <div id="walletInfo" class="flex flex-col items-end">
                <button id="walletBtn" onclick="connectWallet()" class="bg-[#14F195] text-black px-6 py-2 rounded-xl font-black text-[10px] uppercase shadow-lg shadow-[#14F195]/20">Sync Wallet</button>
                <p id="userBalance" class="text-[9px] font-bold text-[#14F195] mt-1 hidden"></p>
            </div>
        </div>
    </nav>

    <main class="pt-32 px-6 max-w-7xl mx-auto grid lg:grid-cols-12 gap-10">
        
        <div class="lg:col-span-4 space-y-6">
            <div class="glass p-6 rounded-3xl border-white/10">
                <p class="text-[8px] text-gray-500 font-bold uppercase mb-4 tracking-widest text-center">Global Population Census</p>
                <div class="grid grid-cols-2 gap-4 text-center">
                    <div class="bg-black/40 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🐀 Rats</p>
                        <p id="count-Common" class="text-sm font-black text-gray-500">0</p>
                    </div>
                    <div class="bg-black/40 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🦈 Sharks</p>
                        <p id="count-Rare" class="text-sm font-black text-blue-400">0</p>
                    </div>
                    <div class="bg-black/40 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🔥 Phoenix</p>
                        <p id="count-Legendary" class="text-sm font-black text-pink-500">0</p>
                    </div>
                    <div class="bg-black/40 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🛸 Overlords</p>
                        <p id="count-Mythic" class="text-sm font-black text-yellow-400">0</p>
                    </div>
                </div>
            </div>

            <div class="glass p-10 rounded-[3rem] text-center border-white/10 relative overflow-hidden">
                <div class="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-transparent via-[#14F195] to-transparent opacity-30"></div>
                <div id="egg" class="text-9xl mb-12 select-none drop-shadow-2xl">🥚</div>
                <div class="flex flex-col gap-3">
                    <button id="hatchBtn" onclick="payAndHatch('SOL')" class="w-full py-5 bg-white text-black rounded-2xl font-black uppercase text-[10px] hover:bg-[#14F195] transition-colors">Hatch DNA (0.006 SOL)</button>
                    <button id="tokenBtn" onclick="payAndHatch('TOKEN')" class="w-full py-5 bg-[#14F195]/10 text-[#14F195] border border-[#14F195]/30 rounded-2xl font-black uppercase text-[10px] hover:bg-[#14F195] hover:text-black transition-all">Hatch (100K $ESHART)</button>
                </div>
                <p id="statusMsg" class="mt-4 text-[7px] text-gray-500 font-bold uppercase tracking-widest">System Online</p>
            </div>
        </div>

        <div class="lg:col-span-8">
            <h2 class="text-4xl font-heading font-black uppercase italic mb-10 text-white/90">Asset <span class="shart-gradient">Vault</span></h2>
            <div id="zooGrid" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>
        </div>
    </main>

    <script>
        window.Buffer = window.buffer.Buffer;
        const TREASURY = "6oTWGoVChNbPPuoz5J6cJjmqqBrRcvW9myvyuwnk5NQw";
        const TOKEN_MINT = "t9uEjUYugPdhLv9NHMR3CB6s3HZRM1xbaT2wDKvpump";
        const RPC = "https://mainnet.helius-rpc.com/?api-key=b4f863b0-2443-499c-bc1c-c612f8d7c249";
        const NAMESPACE = "eshart_zoo_v18"; 
        
        let wallet = null;

        const SPECIES = [
            { name: "Sewer Rat", img: "🐀", rarity: "Common", floor: 0.003 },
            { name: "Neon Shark", img: "🦈", rarity: "Rare", floor: 0.015 },
            { name: "Phoenix", img: "🔥", rarity: "Legendary", floor: 0.075 },
            { name: "Elon Overlord", img: "🛸", rarity: "Mythic", floor: 0.500 }
        ];

        async function init() {
            syncCounters();
        }

        async function syncCounters() {
            ["Common", "Rare", "Legendary", "Mythic"].forEach(async (rarity) => {
                try {
                    const hRes = await fetch(`https://api.countapi.xyz/get/${NAMESPACE}/${rarity}_hatched`);
                    const bRes = await fetch(`https://api.countapi.xyz/get/${NAMESPACE}/${rarity}_burned`);
                    const hData = await hRes.json();
                    const bData = await bRes.json();
                    document.getElementById(`count-${rarity}`).innerText = (hData.value - (bData.value || 0)).toLocaleString();
                } catch (e) {}
            });
        }

        // NEW: FETCH TOKEN BALANCE
        async function updateBalance() {
            if (!wallet) return;
            try {
                const conn = new solanaWeb3.Connection(RPC, "confirmed");
                const mint = new solanaWeb3.PublicKey(TOKEN_MINT);
                const owner = new solanaWeb3.PublicKey(wallet);
                const SPL = window.splToken || window.solanaSplToken;
                
                const userATA = await SPL.getAssociatedTokenAddress(mint, owner);
                const balanceInfo = await conn.getTokenAccountBalance(userATA);
                
                const bal = balanceInfo.value.uiAmount;
                const balEl = document.getElementById('userBalance');
                balEl.innerText = bal.toLocaleString() + " $ESHART";
                balEl.classList.remove('hidden');
            } catch (e) {
                document.getElementById('userBalance').innerText = "0 $ESHART";
                document.getElementById('userBalance').classList.remove('hidden');
            }
        }

        async function connectWallet() {
            if (!window.solana) return alert("Use Phantom Browser!");
            const resp = await window.solana.connect();
            wallet = resp.publicKey.toString();
            document.getElementById('walletBtn').innerText = wallet.slice(0,6) + "...";
            render();
            syncCounters();
            updateBalance(); // Get balance on connect
        }

        async function payAndHatch(type) {
            if (!wallet) return alert("Sync Wallet!");
            const btn = document.getElementById(type === 'SOL' ? 'hatchBtn' : 'tokenBtn');
            const status = document.getElementById('statusMsg');
            btn.disabled = true;
            btn.innerText = "SIGNING...";

            try {
                const conn = new solanaWeb3.Connection(RPC, "confirmed");
                const from = new solanaWeb3.PublicKey(wallet);
                const to = new solanaWeb3.PublicKey(TREASURY);
                const tx = new solanaWeb3.Transaction();

                if (type === 'TOKEN') {
                    const SPL = window.splToken || window.solanaSplToken;
                    const mint = new solanaWeb3.PublicKey(TOKEN_MINT);
                    const userATA = await SPL.getAssociatedTokenAddress(mint, from);
                    const treasuryATA = await SPL.getAssociatedTokenAddress(mint, to);
                    
                    const acc = await conn.getAccountInfo(treasuryATA);
                    if (!acc) tx.add(SPL.createAssociatedTokenAccountInstruction(from, treasuryATA, to, mint));
                    tx.add(SPL.createTransferInstruction(userATA, treasuryATA, from, 100000 * Math.pow(10, 6)));
                } else {
                    tx.add(solanaWeb3.SystemProgram.transfer({ fromPubkey: from, toPubkey: to, lamports: 0.006 * solanaWeb3.LAMPORTS_PER_SOL }));
                }

                const { blockhash } = await conn.getLatestBlockhash();
                tx.recentBlockhash = blockhash;
                tx.feePayer = from;

                const { signature } = await window.solana.signAndSendTransaction(tx);
                status.innerText = "HATCHING...";
                await conn.confirmTransaction(signature);
                finalize();
                updateBalance(); // Update balance after spend
            } catch (e) {
                alert("Transaction Cancelled.");
            } finally {
                btn.disabled = false;
                btn.innerText = type === 'SOL' ? "Hatch (SOL)" : "Hatch ($ESHART)";
                status.innerText = "System Online";
            }
        }

        function finalize() {
            const r = Math.random() * 100;
            let rarity = "Common";
            if (r > 99.8) rarity = "Mythic";
            else if (r > 97.8) rarity = "Legendary";
            else if (r > 90) rarity = "Rare";

            fetch(`https://api.countapi.xyz/hit/${NAMESPACE}/${rarity}_hatched`);
            const animal = SPECIES.find(s => s.rarity === rarity);
            const zooData = JSON.parse(localStorage.getItem(`zoo_v28_${wallet}`) || "[]");
            zooData.push({ id: Date.now(), ...animal, rarity });
            localStorage.setItem(`zoo_v28_${wallet}`, JSON.stringify(zooData));
            render();
            syncCounters();
        }

        function render() {
            const grid = document.getElementById('zooGrid'); grid.innerHTML = '';
            const data = JSON.parse(localStorage.getItem(`zoo_v28_${wallet}`) || "[]");
            data.forEach(a => {
                const d = document.createElement('div');
                d.className = `glass p-8 rounded-3xl card-${a.rarity} text-center`;
                d.innerHTML = `<div class="text-7xl mb-4 drop-shadow-lg">${a.img}</div>
                <p class="text-[9px] font-black opacity-40 uppercase">${a.rarity}</p>
                <p class="text-xs font-black text-sky-400">Burn Value: ${a.floor} SOL</p>
                <button onclick="burn(${a.id}, '${a.rarity}')" class="mt-6 w-full py-2 bg-red-500/10 text-red-500 rounded-xl font-black uppercase text-[7px] hover:bg-red-500 hover:text-white transition-all">Burn (50%)</button>`;
                grid.appendChild(d);
            });
        }

        async function burn(id, rarity) {
            if (confirm("Burn asset and update Census?")) {
                await fetch(`https://api.countapi.xyz/hit/${NAMESPACE}/${rarity}_burned`);
                const data = JSON.parse(localStorage.getItem(`zoo_v28_${wallet}`) || "[]");
                localStorage.setItem(`zoo_v28_${wallet}`, JSON.stringify(data.filter(a => a.id !== id)));
                render();
                syncCounters();
            }
        }

        init();
    </script>
</body>
</html>
