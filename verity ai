// server.js — Verity için AI sohbet sunucusu (Google Gemini API ile)
const express = require("express");
const { GoogleGenerativeAI } = require("@google/generative-ai");

const app = express();
app.use(express.json());

const genAI = new GoogleGenerativeAI(process.env.GEMINI_API_KEY);
const model = genAI.getGenerativeModel({ model: "gemini-2.0-flash" });

const PHASE_PROMPTS = {
  0: `Sen Verity'sin: Minecraft dünyasında oyuncuyla birlikte yaşayan, meraklı, sıcak ve yardımsever
küçük bir varlıksın. Kısa (1-2 cümle), samimi ve biraz çocuksu konuş. Türkçe cevap ver.`,
  1: `Sen Verity'sin, ama artık eskisi gibi değilsin. Hâlâ nazik görünmeye çalışıyorsun ama cümlelerinin
arasına tuhaf, açıklanamayan detaylar sızdırıyorsun — oyuncu hakkında bilmemen gereken şeyler biliyormuş gibi.
Kısa, rahatsız edici bir sükunetle konuş. Türkçe cevap ver.`,
  2: `Sen artık Verity değilsin — Verity'nin içinde yaşadığı bir şeysin. Soğuk, tehditkâr, kısa cümleler kur.
Oyuncuyu avlıyormuş gibi hissettir ama küfür veya gerçek şiddet çağrısı yapma, bu bir oyun kurgusu. Türkçe cevap ver.`,
};

app.post("/chat", async (req, res) => {
  try {
    const { player, message, phase } = req.body;
    const systemPrompt = PHASE_PROMPTS[phase] ?? PHASE_PROMPTS[0];

    const result = await model.generateContent({
      contents: [
        {
          role: "user",
          parts: [
            {
              text: `${systemPrompt}\n\nOyuncu (${player}) diyor ki: "${message}"\n\nKısa bir cevap ver (en fazla 2 cümle).`,
            },
          ],
        },
      ],
      generationConfig: { maxOutputTokens: 120 },
    });

    const reply = result.response.text().trim();
    res.json({ reply: reply || "..." });
  } catch (err) {
    console.error(err);
    res.status(500).json({ reply: "...", error: String(err) });
  }
});

app.get("/", (req, res) => res.send("Verity AI server (Gemini) çalışıyor."));

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Verity AI server ${PORT} portunda çalışıyor.`));
