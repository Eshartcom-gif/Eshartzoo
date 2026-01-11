
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ESHART ZOO | V29 Genesis Pin</title>
    
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
            background-image: linear-gradient(rgba(1, 2, 4, 0.9), rgba(1, 2, 4, 0.9)), url('https://images.unsplash.com/photo-1639762681485-074b7f938ba0?q=80&w=2832&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }
        .font-heading { font-family: 'Orbitron', sans-serif; }
        .glass { background: rgba(255, 255, 255, 0.02); border: 1px solid rgba(255, 255, 255, 0.05); backdrop-filter: blur(20px); }
        .shart-gradient { background: linear-gradient(90deg, #9945FF, #14F195); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        
        /* THE TELEGRAM PIN */
        .tg-pin {
            position: fixed;
            bottom: 30px;
            right: 30px;
            z-index: 1000;
            background: #0088cc;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 0 20px rgba(0, 136, 204, 0.5);
            transition: transform 0.3s ease;
        }
        .tg-pin:hover { transform: scale(1.1) rotate(10deg); }

        .card-Common { border-left: 3px solid #475569; }
        .card-Mythic { border-left: 3px solid #fbbf24; border-right: 3px solid #fbbf24; box-shadow: 0 0 30px #fbbf2444; }
    </style>
</head>
<body class="min-h-screen pb-20">

    <a href="https://t.me/ElonSHARTS" target="_blank" class="tg-pin">
        <svg width="30" height="30" viewBox="0 0 24 24" fill="white">
            <path d="M12 0c-6.627 0-12 5.373-12 12s5.373 12 12 12 12-5.373 12-12-5.373-12-12-12zm4.462 15.998l-.017.012-3.469 1.912c-.272.149-.599.045-.747-.227l-1.012-1.874-1.712.851-.001.001-.001.001c-.662.333-1.42-.147-1.42-.888v-4.52l-2.062-1.031c-.272-.137-.382-.464-.245-.736s.464-.382.736-.245l11.161 5.58c.272.137.382.464.245.736s-.463.382-.736.241z"/>
        </svg>
    </a>

    <nav class="fixed top-0 w-full z-[100] glass border-b border-white/5 px-6 py-4 flex justify-between items-center">
        <div class="flex items-center gap-3">
            <img src="https://i.ibb.co/0jdXM3fZ/1767978719058-2.jpg" class="h-10 w-10 rounded-lg">
            <span class="text-xl font-black font-heading shart-gradient uppercase italic">ESHART ZOO</span>
        </div>
        
        <div id="walletSection" class="text-right">
            <button id="walletBtn" onclick="connectWallet()" class="bg-[#14F195] text-black px-6 py-2 rounded-xl font-black text-[10px] uppercase shadow-lg shadow-[#14F195]/20">Sync Wallet</button>
            <div id="balances" class="hidden mt-1 space-y-0.5">
                <p id="solBal" class="text-[8px] font-bold text-gray-400"></p>
                <p id="tokenBal" class="text-[8px] font-bold text-[#14F195]"></p>
            </div>
        </div>
    </nav>

    <main class="pt-32 px-6 max-w-7xl mx-auto grid lg:grid-cols-12 gap-10">
        
        <div class="lg:col-span-4 space-y-6">
            <div class="glass p-6 rounded-3xl border-white/10">
                <p class="text-[8px] text-gray-500 font-bold uppercase mb-4 tracking-widest text-center">Global Population Census</p>
                <div class="grid grid-cols-2 gap-4 text-center">
                    <div class="bg-black/30 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🐀 Rats</p>
                        <p id="count-Common" class="text-lg font-black text-gray-500">0</p>
                    </div>
                    <div class="bg-black/30 p-3 rounded-xl border border-white/5">
                        <p class="text-[7px] text-gray-400 uppercase">🛸 Overlords</p>
                        <p id="count-Mythic" class="text-lg font-black text-yellow-400">0</p>
                    </div>
                </div>
            </div>

            <div class="glass p-10 rounded-[3rem] text-center border-white/10">
                <div id="egg" class="text-9xl mb-12 select-none drop-shadow-2xl">🥚</div>
                <button id="hatchBtn" onclick="payAndHatch()" class="w-full py-6 bg-white text-black rounded-2xl font-black uppercase text-[12px] shadow-xl hover:bg-[#14F195] transition-all">Hatch DNA (0.006 SOL)</button>
                <p id="statusMsg" class="mt-4 text-[7px] text-gray-600 font-bold uppercase tracking-widest italic">Solana Mainnet Protocol</p>
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

        async function init() { syncCounters(); }

        async function syncCounters() {
            ["Common", "Mythic"].forEach(async (rarity) => {
                try {
                    const hRes = await fetch(`https://api.countapi.xyz/get/${NAMESPACE}/${rarity}_hatched`);
                    const bRes = await fetch(`https://api.countapi.xyz/get/${NAMESPACE}/${rarity}_burned`);
                    const hData = await hRes.json(); const bData = await bRes.json();
                    document.getElementById(`count-${rarity}`).innerText = (hData.value - (bData.value || 0)).toLocaleString();
                } catch (e) {}
            });
        }

        async function updateBalances() {
            if (!wallet) return;
            try {
                const conn = new solanaWeb3.Connection(RPC, "confirmed");
                const pubkey = new solanaWeb3.PublicKey(wallet);
                
                // 1. SOL Balance
                const sol = await conn.getBalance(pubkey);
                document.getElementById('solBal').innerText = (sol / 1e9).toFixed(3) + " SOL";
                
                // 2. Token Balance
                const SPL = window.splToken || window.solanaSplToken;
                const userATA = await SPL.getAssociatedTokenAddress(new solanaWeb3.PublicKey(TOKEN_MINT), pubkey);
                const tokenInfo = await conn.getTokenAccountBalance(userATA);
                document.getElementById('tokenBal').innerText = tokenInfo.value.uiAmount.toLocaleString() + " $ESHART";
                
                document.getElementById('balances').classList.remove('hidden');
            } catch (e) {
                document.getElementById('tokenBal').innerText = "0 $ESHART";
                document.getElementById('balances').classList.remove('hidden');
            }
        }

        async function connectWallet() {
            if (!window.solana) return alert("Open in Phantom App!");
            const resp = await window.solana.connect();
            wallet
