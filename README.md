
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ESHART ZOO | V27 Solid State</title>
    
    <script src="https://bundle.run/buffer@6.0.3"></script>
    <script src="https://unpkg.com/@solana/web3.js@1.95.3/lib/index.iife.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@solana/spl-token@0.3.9/lib/index.iife.min.js"></script>
    
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
                    <div class="bg-white/5 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🐀 Rats</p>
                        <p id="count-Common" class="text-sm font-black text-gray-500">0</p>
                    </div>
                    <div class="bg-white/5 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🦈 Sharks</p>
                        <p id="count-Rare" class="text-sm font-black text-blue-400">0</p>
                    </div>
                    <div class="bg-white/5 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🔥 Phoenix</p>
                        <p id="count-Legendary" class="text-sm font-black text-pink-500">0</p>
                    </div>
                    <div class="bg-white/5 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🛸 Overlords</p>
                        <p id="count-Mythic" class="text-sm font-black text-yellow-400">0</p>
                    </div>
                </div>
            </div>

            <div class="glass p-10 rounded-[3rem] text-center border-white/10">
                <div id="egg" class="text-9xl mb-12 select-none">🥚</div>
                <div class="flex flex-col gap-3">
                    <button id="hatchBtn" onclick="payAndHatch('SOL')" class="w-full py-5 bg-white text-black rounded-2xl font-black uppercase text-[10px]">Hatch DNA (0.006 SOL)</button>
                    <button id="tokenBtn" onclick="payAndHatch('TOKEN')" class="w-full py-5 bg-[#14F195]/10 text-[#14F195] border border-[#14F195]/30 rounded-2xl font-black uppercase text-[10px]">Hatch (100K $ESHART)</button>
                </div>
                <p id="statusMsg" class="mt-4 text-[7px] text-gray-600 font-bold uppercase tracking-widest">Protocol Verified</p>
            </div>
        </div>

        <div class="lg:col-span-8">
            <h2 class="text-4xl font-heading font-black uppercase italic mb-10 text-white/90">The <span class="shart-gradient">Vault</span></h2>
            <div id="zooGrid" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>
        </div>
    </main>

    <script>
        // 2. CORE LOGIC
        window.Buffer = window.buffer.Buffer;
        const TREASURY = "6oTWGoVChNbPPuoz5J6cJjmqqBrRcvW9myvyuwnk5NQw";
        const TOKEN_MINT = "t9uEjUYugPdhLv9NHMR3CB6s3HZRM1xbaT2wDKvpump";
        const RPC = "https://mainnet.helius-rpc.com/?api-key=b4f863b0-2443-499c-bc1c-c612f8d7c249";
        const NAMESPACE = "eshart_zoo_v18"; // LEGACY PULSE
        
        let wallet = null;
        let zoo = [];

        const SPECIES = [
            { name: "Sewer Rat", img: "🐀", rarity: "Common", floor: 0.003 },
            { name: "Neon Shark", img: "🦈", rarity: "Rare", floor: 0.015 },
            { name: "Phoenix", img: "🔥", rarity: "Legendary", floor: 0.075 },
            { name: "Elon Overlord", img: "🛸", rarity: "Mythic", floor: 0.500 }
        ];

        // 3. INITIALIZATION
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

        async function connectWallet() {
            if (!window.solana) return alert("Use Phantom Browser!");
            const resp = await window.solana.connect();
            wallet = resp.publicKey.toString();
            document.getElementById('walletBtn').innerText = wallet.slice(0,6) + "...";
            render();
            syncCounters();
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
                    // 100,000 $ESHART (assuming 6 decimals)
                    tx.add(SPL.createTransferInstruction(userATA, treasuryATA, from, 100000 * Math.pow(10, 6)));
                } else {
                    tx.add(solanaWeb3.SystemProgram.transfer({ fromPubkey: from, toPubkey: to, lamports: 0.006 * solanaWeb3.LAMPORTS_PER_SOL }));
                }

                // --- 2026 MANDATORY UPDATE: Get context for Phantom ---
                const { blockhash, lastValidBlockHeight } = await conn.getLatestBlockhash();
                tx.recentBlockhash = blockhash;
                tx.feePayer = from;

                const { signature } = await window.solana.signAndSendTransaction(tx);
                status.innerText = "CONFIRMING...";
                await conn.confirmTransaction(signature);
                finalize();
            } catch (e) {
                alert("Transaction Cancelled or Failed.");
            } finally {
                btn.disabled = false;
                btn.innerText = type === 'SOL' ? "Hatch (SOL)" : "Hatch ($ESHART)";
                status.innerText = "Protocol Verified";
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
            const zooData = JSON.parse(localStorage.getItem(`zoo_v27_${wallet}`) || "[]");
            zooData.push({ id: Date.now(), ...animal, rarity });
            localStorage.setItem(`zoo_v27_${wallet}`, JSON.stringify(zooData));
            render();
            syncCounters();
        }

        function render() {
            const grid = document.getElementById('zooGrid'); grid.innerHTML = '';
            const data = JSON.parse(localStorage.getItem(`zoo_v27_${wallet}`) || "[]");
            data.forEach(a => {
                const d = document.createElement('div');
                d.className = `glass p-8 rounded-3xl card-${a.rarity} text-center`;
                d.innerHTML = `<div class="text-7xl mb-4">${a.img}</div><p class="text-[9px] font-black opacity-40 uppercase">${a.rarity}</p>
                <p class="text-xs font-black text-sky-400">Burn: ${a.floor} SOL</p>
                <button onclick="burn(${a.id}, '${a.rarity}')" class="mt-6 w-full py-2 bg-red-500/10 text-red-500 rounded-xl font-black uppercase text-[7px]">Burn (50%)</button>`;
                grid.appendChild(d);
            });
        }

        async function burn(id, rarity) {
            if (confirm("Burn asset and update Census?")) {
                await fetch(`https://api.countapi.xyz/hit/${NAMESPACE}/${rarity}_burned`);
                const data = JSON.parse(localStorage.getItem(`zoo_v27_${wallet}`) || "[]");
                localStorage.setItem(`zoo_v27_${wallet}`, JSON.stringify(data.filter(a => a.id !== id)));
                render();
                syncCounters();
            }
        }

        init();
    </script>
</body>
</html>
