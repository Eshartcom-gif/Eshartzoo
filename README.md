
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ESHART ZOO | V30 Immortal</title>
    
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
            /* CYBER GRID BACKGROUND */
            background-image: 
                linear-gradient(rgba(1, 2, 4, 0.93), rgba(1, 2, 4, 0.93)), 
                url('https://images.unsplash.com/photo-1635070041078-e363dbe005cb?q=80&w=2070&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }
        .font-heading { font-family: 'Orbitron', sans-serif; }
        .glass { background: rgba(255, 255, 255, 0.02); border: 1px solid rgba(255, 255, 255, 0.08); backdrop-filter: blur(15px); }
        .shart-gradient { background: linear-gradient(90deg, #9945FF, #14F195); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        
        /* TELEGRAM FLOATING PIN */
        .tg-pin {
            position: fixed;
            bottom: 25px;
            right: 25px;
            z-index: 1000;
            background: #0088cc;
            width: 55px;
            height: 55px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 0 25px rgba(0, 136, 204, 0.6);
            animation: pulse 2s infinite;
        }
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(0, 136, 204, 0.7); } 70% { box-shadow: 0 0 0 15px rgba(0, 136, 204, 0); } 100% { box-shadow: 0 0 0 0 rgba(0, 136, 204, 0); } }

        .card-Mythic { border-left: 3px solid #fbbf24; border-right: 3px solid #fbbf24; box-shadow: 0 0 40px rgba(251, 191, 36, 0.2); }
    </style>
</head>
<body class="min-h-screen">

    <a href="https://t.me/ElonSHARTS" target="_blank" class="tg-pin">
        <svg width="28" height="28" viewBox="0 0 24 24" fill="white"><path d="M12 0c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm4.462 15.998l-.017.012-3.469 1.912c-.272.149-.599.045-.747-.227l-1.012-1.874-1.712.851-.001.001-.001.001c-.662.333-1.42-.147-1.42-.888v-4.52l-2.062-1.031c-.272-.137-.382-.464-.245-.736s.464-.382.736-.245l11.161 5.58c.272.137.382.464.245.736s-.463.382-.736.241z"/></svg>
    </a>

    <nav class="fixed top-0 w-full z-[100] glass border-b border-white/5 px-6 py-4 flex justify-between items-center">
        <div class="flex items-center gap-3">
            <img src="https://i.ibb.co/0jdXM3fZ/1767978719058-2.jpg" class="h-10 w-10 rounded-lg">
            <span class="text-xl font-black font-heading shart-gradient uppercase italic">ESHART ZOO</span>
        </div>
        
        <div id="walletDisplay" class="text-right">
            <button id="walletBtn" onclick="connectWallet()" class="bg-[#14F195] text-black px-6 py-2 rounded-xl font-black text-[10px] uppercase shadow-lg shadow-[#14F195]/20">Sync Wallet</button>
            <div id="userStats" class="hidden mt-1 text-[8px] font-bold space-y-0.5">
                <p id="solBalance" class="text-gray-400">0.00 SOL</p>
                <p id="tokenBalance" class="text-[#14F195]">0 $ESHART</p>
            </div>
        </div>
    </nav>

    <main class="pt-32 px-6 max-w-7xl mx-auto grid lg:grid-cols-12 gap-10">
        <div class="lg:col-span-4 space-y-6">
            <div class="glass p-6 rounded-3xl border-white/10">
                <p class="text-[8px] text-gray-500 font-bold uppercase mb-4 tracking-widest text-center">Global Pulse (Stable)</p>
                <div class="grid grid-cols-2 gap-4 text-center">
                    <div class="bg-black/40 p-3 rounded-xl"><p class="text-[7px] text-gray-400 uppercase">🐀 Rats</p><p id="count-Common" class="text-lg font-black text-gray-500">4,192</p></div>
                    <div class="bg-black/40 p-3 rounded-xl"><p class="text-[7px] text-gray-400 uppercase">🛸 Overlords</p><p id="count-Mythic" class="text-lg font-black text-yellow-400">12</p></div>
                </div>
            </div>

            <div class="glass p-10 rounded-[3rem] text-center border-white/10 relative">
                <div id="egg" class="text-9xl mb-12 select-none drop-shadow-2xl">🥚</div>
                <button id="hatchBtn" onclick="payAndHatch()" class="w-full py-6 bg-white text-black rounded-2xl font-black uppercase text-[11px] shadow-xl hover:bg-[#14F195] transition-all">Hatch DNA (0.006 SOL)</button>
                <p id="statusMsg" class="mt-4 text-[7px] text-gray-600 font-bold uppercase tracking-widest">Protocol: Active</p>
            </div>
        </div>

        <div class="lg:col-span-8">
            <h2 class="text-4xl font-heading font-black uppercase italic mb-10 text-white/90">The <span class="shart-gradient">Vault</span></h2>
            <div id="zooGrid" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>
        </div>
    </main>

    <script>
        window.Buffer = window.buffer.Buffer;
        const TREASURY = "6oTWGoVChNbPPuoz5J6cJjmqqBrRcvW9myvyuwnk5NQw";
        const TOKEN_MINT = "t9uEjUYugPdhLv9NHMR3CB6s3HZRM1xbaT2wDKvpump";
        const RPC = "https://mainnet.helius-rpc.com/?api-key=b4f863b0-2443-499c-bc1c-c612f8d7c249";
        
        let wallet = null;

        async function updateStats() {
            if (!wallet) return;
            try {
                const conn = new solanaWeb3.Connection(RPC, "confirmed");
                const pubkey = new solanaWeb3.PublicKey(wallet);
                
                const sol = await conn.getBalance(pubkey);
                document.getElementById('solBalance').innerText = (sol / 1e9).toFixed(3) + " SOL";

                const SPL = window.splToken || window.solanaSplToken;
                const userATA = await SPL.getAssociatedTokenAddress(new solanaWeb3.PublicKey(TOKEN_MINT), pubkey);
                const tokenInfo = await conn.getTokenAccountBalance(userATA);
                document.getElementById('tokenBalance').innerText = tokenInfo.value.uiAmount.toLocaleString() + " $ESHART";
                
                document.getElementById('userStats').classList.remove('hidden');
            } catch (e) { document.getElementById('userStats').classList.remove('hidden'); }
        }

        async function connectWallet() {
            if (!window.solana) return alert("Open in Phantom Browser!");
            const resp = await window.solana.connect();
            wallet = resp.publicKey.toString();
            document.getElementById('walletBtn').innerText = wallet.slice(0,6) + "...";
            updateStats(); render();
        }

        async function payAndHatch() {
            if (!wallet) return alert("Connect Wallet!");
            const btn = document.getElementById('hatchBtn');
            const status = document.getElementById('statusMsg');
            
            btn.disabled = true; btn.innerText = "SIGNING...";
            status.innerText = "AUTHENTICATING ON-CHAIN...";

            try {
                const conn = new solanaWeb3.Connection(RPC, "confirmed");
                const from = new solanaWeb3.PublicKey(wallet);
                
                const tx = new solanaWeb3.Transaction().add(
                    solanaWeb3.SystemProgram.transfer({ fromPubkey: from, toPubkey: new solanaWeb3.PublicKey(TREASURY), lamports: 0.006 * 1e9 })
                );

                const { blockhash } = await conn.getLatestBlockhash();
                tx.recentBlockhash = blockhash;
                tx.feePayer = from;

                const { signature } = await window.solana.signAndSendTransaction(tx);
                status.innerText = "HATCHING ASSET...";
                await conn.confirmTransaction(signature);
                
                finalizeHatch();
            } catch (e) { alert("Tx Cancelled."); } finally { 
                btn.disabled = false; btn.innerText = "Hatch DNA (0.006 SOL)"; 
                status.innerText = "Protocol: Active";
                updateStats();
            }
        }

        function finalizeHatch() {
            const r = Math.random() * 100;
            const rarity = r > 99.8 ? "Mythic" : "Common";
            const animal = rarity === "Mythic" ? { name: "Elon Overlord", img: "🛸", floor: 0.500 } : { name: "Sewer Rat", img: "🐀", floor: 0.003 };
            
            const zoo = JSON.parse(localStorage.getItem(`zoo_v30_${wallet}`) || "[]");
            zoo.push({ id: Date.now(), ...animal, rarity });
            localStorage.setItem(`zoo_v30_${wallet}`, JSON.stringify(zoo));
            render();
        }

        function render() {
            const grid = document.getElementById('zooGrid'); grid.innerHTML = '';
            const data = JSON.parse(localStorage.getItem(`zoo_v30_${wallet}`) || "[]");
            data.forEach(a => {
                grid.innerHTML += `<div class="glass p-8 rounded-3xl card-${a.rarity} text-center transition-all hover:scale-105"><div class="text-7xl mb-4">${a.img}</div><p class="text-[10px] text-sky-400 font-black">Burn: ${a.floor} SOL</p></div>`;
            });
        }
    </script>
</body>
</html>
