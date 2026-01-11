
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ESHART ZOO | V19.1 SOL-Only</title>
    
    <script src="https://bundle.run/buffer@6.0.3"></script>
    <script src="https://unpkg.com/@solana/web3.js@1.95.3/lib/index.iife.min.js"></script>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@900&family=Inter:wght@400;700&display=swap" rel="stylesheet">
    
    <style>
        :root { --accent: #14F195; --bg: #010204; }
        body { background: var(--bg); color: white; font-family: 'Inter', sans-serif; margin: 0; }
        .font-heading { font-family: 'Orbitron', sans-serif; }
        .glass { background: rgba(255, 255, 255, 0.01); border: 1px solid rgba(255, 255, 255, 0.05); backdrop-filter: blur(20px); }
        .shart-gradient { background: linear-gradient(90deg, #9945FF, #14F195); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .card-Common { border-left: 3px solid #475569; }
        .card-Rare { border-left: 3px solid #3b82f6; }
        .card-Legendary { border-left: 3px solid #ff00ff; }
        .card-Mythic { border-left: 3px solid #fbbf24; border-right: 3px solid #fbbf24; box-shadow: 0 0 30px #fbbf2444; }
    </style>
</head>
<body class="min-h-screen pb-20">

    <nav class="fixed top-0 w-full z-[100] glass border-b border-white/5 px-6 py-4 flex justify-between items-center">
        <div class="flex items-center gap-3">
            <img src="https://i.ibb.co/0jdXM3fZ/1767978719058-2.jpg" class="h-10 w-10 rounded-lg">
            <span class="text-xl font-black font-heading shart-gradient uppercase italic">ESHART ZOO</span>
        </div>
        <button id="walletBtn" onclick="connectWallet()" class="bg-[#14F195] text-black px-6 py-2 rounded-xl font-black text-[10px] uppercase">Sync Wallet</button>
    </nav>

    <main class="pt-32 px-6 max-w-7xl mx-auto grid lg:grid-cols-12 gap-10">
        
        <div class="lg:col-span-4 space-y-6">
            <div class="glass p-6 rounded-3xl border-white/10">
                <p class="text-[8px] text-gray-500 font-bold uppercase mb-4 tracking-widest text-center">Global Live Census</p>
                <div class="grid grid-cols-2 gap-4 text-center">
                    <div class="bg-white/5 p-3 rounded-xl">
                        <p class="text-[7px] text-gray-400 uppercase">🐀 Rats</p>
                        <p id="count-Common" class="text-lg font-black text-gray-500">0</p>
                    </div>
                    <div class="bg-white/5 p-3 rounded-xl">
                        <p class="text-[7px] text-gray-400 uppercase">🦈 Sharks</p>
                        <p id="count-Rare" class="text-lg font-black text-blue-400">0</p>
                    </div>
                    <div class="bg-white/5 p-3 rounded-xl">
                        <p class="text-[7px] text-gray-400 uppercase">🔥 Phoenix</p>
                        <p id="count-Legendary" class="text-lg font-black text-pink-500">0</p>
                    </div>
                    <div class="bg-white/5 p-3 rounded-xl">
                        <p class="text-[7px] text-gray-400 uppercase">🛸 Overlords</p>
                        <p id="count-Mythic" class="text-lg font-black text-yellow-400">0</p>
                    </div>
                </div>
            </div>

            <div class="glass p-6 rounded-3xl border-[#14F195]/20 bg-[#14F195]/5">
                <div class="flex justify-between">
                    <span class="text-[10px] text-gray-400 uppercase font-bold tracking-widest">SOL Market Price</span>
                    <span id="solPrice" class="text-[10px] font-black font-mono tracking-tighter">---</span>
                </div>
            </div>

            <div class="glass p-10 rounded-[3rem] text-center border-white/10">
                <div id="egg" class="text-9xl mb-12 select-none">🥚</div>
                <button id="hatchBtn" onclick="payAndHatch()" class="w-full py-5 bg-white text-black rounded-2xl font-black uppercase text-[10px] tracking-widest hover:bg-[#14F195] transition">Hatch DNA (0.006 SOL)</button>
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
        const RPC = "https://mainnet.helius-rpc.com/?api-key=b4f863b0-2443-499c-bc1c-c612f8d7c249";
        const NAMESPACE = "eshart_zoo_master_v19_sol";
        
        let wallet = null, zoo = [];

        async function init() {
            fetchPrices(); syncCounters();
            setInterval(fetchPrices, 5000); 
        }

        async function fetchPrices() {
            try {
                const res = await fetch(`https://api.dexscreener.com/latest/dex/tokens/So11111111111111111111111111111111111111112`);
                const data = await res.json();
                const solPair = data.pairs.find(p => p.baseToken.address === "So11111111111111111111111111111111111111112");
                if (solPair) {
                    document.getElementById('solPrice').innerText = `$${parseFloat(solPair.priceUsd).toFixed(2)}`;
                }
            } catch (e) {}
        }

        async function syncCounters() {
            ["Common", "Rare", "Legendary", "Mythic"].forEach(async (rarity) => {
                try {
                    const hRes = await fetch(`https://api.countapi.xyz/get/${NAMESPACE}/${rarity}_hatched`);
                    const bRes = await fetch(`https://api.countapi.xyz/get/${NAMESPACE}/${rarity}_burned`);
                    const hD = await hRes.json(); const bD = await bRes.json();
                    document.getElementById(`count-${rarity}`).innerText = (hD.value - (bD.value || 0)).toLocaleString();
                } catch (e) {}
            });
        }

        async function connectWallet() {
            if (!window.solana) return alert("Use Phantom Browser!");
            const resp = await window.solana.connect();
            wallet = resp.publicKey.toString();
            document.getElementById('walletBtn').innerText = wallet.slice(0,6) + "...";
            render(); syncCounters();
        }

        async function payAndHatch() {
            if (!wallet) return alert("Sync Wallet!");
            const btn = document.getElementById('hatchBtn');
            btn.disabled = true; btn.innerText = "AUTHENTICATING...";
            try {
                const conn = new solanaWeb3.Connection(RPC, "confirmed");
                const from = new solanaWeb3.PublicKey(wallet);
                const to = new solanaWeb3.PublicKey(TREASURY);
                const tx = new solanaWeb3.Transaction().add(
                    solanaWeb3.SystemProgram.transfer({ 
                        fromPubkey: from, 
                        toPubkey: to, 
                        lamports: 0.006 * solanaWeb3.LAMPORTS_PER_SOL 
                    })
                );

                const { blockhash } = await conn.getLatestBlockhash();
                tx.recentBlockhash = blockhash; tx.feePayer = from;
                const { signature } = await window.solana.signAndSendTransaction(tx);
                await conn.confirmTransaction(signature);
                finalize();
            } catch (e) { alert("Tx Failed."); } finally { btn.disabled = false; btn.innerText = "Hatch DNA (0.006 SOL)"; }
        }

        function finalize() {
            const r = Math.random() * 100;
            let rarity = r > 99.8 ? "Mythic" : r > 97.8 ? "Legendary" : r > 90 ? "Rare" : "Common";
            fetch(`https://api.countapi.xyz/hit/${NAMESPACE}/${rarity}_hatched`);
            const SPECIES_MAP = {
                Common: { name: "Sewer Rat", img: "🐀", floor: 0.003 },
                Rare: { name: "Neon Shark", img: "🦈", floor: 0.015 },
                Legendary: { name: "Phoenix", img: "🔥", floor: 0.075 },
                Mythic: { name: "Elon Overlord", img: "🛸", floor: 0.500 }
            };
            const animal = SPECIES_MAP[rarity];
            const zoo = JSON.parse(localStorage.getItem(`zoo_v19_sol_${wallet}`) || "[]");
            zoo.push({ id: Date.now(), ...animal, rarity });
            localStorage.setItem(`zoo_v
