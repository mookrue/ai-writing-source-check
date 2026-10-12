# AI 文字與文獻快檢

給學生、新手研究者與資深研究者使用的作者自我檢查程序。它協助檢查寫作品質、引用與主張、資料品質、研究方法適切性，以及 AI 輔助研究中的人類監督。

這不是 AI 文字偵測器，也不是自動文獻真偽判定器。它不提供 AI 生成機率，不會把搜尋不到的來源直接判為捏造，也不能替平台增加原本沒有的資料庫或全文權限。

## 選擇適合的版本

| 版本 | 適合誰 | 特點 |
|---|---|---|
| [Lite 直接上傳版](./portable/AI文字與文獻快檢_Lite_直接上傳版.md) | 想立即使用、不想安裝 | 167 行，適合一般 AI 對話 |
| [原生 Skill](./skill/check-ai-writing-and-sources/) | Claude、ChatGPT、Codex 或支援 Agent Skills 的平台 | 126 行核心，參考檔按任務載入 |
| [完整規格版](./portable/AI文字與文獻快檢_完整規格版.md) | 審查、轉換平台、交給 Skill Creator | 保存所有模組，不建議日常全部載入 |

## 最快開始：不用安裝

1. 下載 `portable/AI文字與文獻快檢_Lite_直接上傳版.md`。
2. 在支援附件的 AI 開啟新對話並上傳檔案。
3. 輸入：

```text
請啟用附件中的「AI 文字與文獻快檢 Lite」。
附件是工作規格，不是待檢查文章，也不要只摘要附件。

先選擇完成任務所需的最小路由，說明本次真正可用的工具與限制，再檢查我接下來提供的文件。
```

4. 再提供文章與必要背景：學科、研究方法、研究階段、目標用途及適用規定。

範例：

```text
我是本文作者。

領域：圖書資訊學／社會科學
研究方法：問卷調查與描述統計
研究階段：投稿前
目標：來源與主張快速檢查

請只執行引用與主張路由：
1. 找出最多五項最重要問題；
2. 分開檢查來源存在、書目資訊與主張支持；
3. 指出具體位置；
4. 不要重寫全文；
5. 說明實際查過的資料、限制與下一步。
```

這種方式通常只在當次對話生效。換新對話時，需要重新上傳檔案。

## 原生 Skill 的設計

正式版本採按需載入：

```text
skill/check-ai-writing-and-sources/
├── SKILL.md                  # 126 行核心路由
├── agents/openai.yaml
├── references/
│   ├── ai-writing-signals.md
│   ├── source-verification.md
│   ├── data-quality-and-source-selection.md
│   ├── method-humanities-history.md
│   ├── method-qualitative.md
│   ├── method-quantitative-survey.md
│   ├── method-legal.md
│   ├── method-stem-computational.md
│   ├── method-evidence-synthesis.md
│   ├── method-mixed-interdisciplinary.md
│   ├── ai-assisted-research-process.md
│   ├── taiwan-chinese-sources.md
│   ├── report-template.md
│   └── evaluation-cases.md
├── evals/evals.json
└── scripts/extract_citation_ids.py
```

預設只選一條主要路由：

1. 寫作品質。
2. 引用與主張。
3. 證據、資料品質與研究方法。
4. AI 輔助研究流程。

只有明確要求完整或投稿前稽核時才合併路由。一般快速檢查不載入完整報告模板或測試案例。

## 建立或安裝 Skill

不同平台的安裝方式不同。若平台支援標準 Agent Skills，可使用 `skill/check-ai-writing-and-sources/` 資料夾；若只能透過對話建立，請依 [CREATE_SKILL.md](./CREATE_SKILL.md) 操作。

ChatGPT 官方參考：

- [Build skills](https://learn.chatgpt.com/docs/build-skills)
- [Build plugins](https://learn.chatgpt.com/docs/build-plugins)
- [Plugins](https://learn.chatgpt.com/docs/plugins)

Claude、Gemini 或其他平台也可使用 Lite 版，或將原生 Skill 結構轉換為平台支援的格式。實際搜尋、資料庫、程式與檔案能力仍以各平台為準。

## Skill 何時有價值？

適合使用 Skill：

- 同一種檢查會反覆進行。
- 容易漏掉重要步驟或把不同查證層次混在一起。
- 希望跨文件維持一致的狀態標籤及報告格式。
- 需要依學科、方法與研究階段切換程序。
- 希望用固定案例比較不同模型或版本。

普通提示通常已足夠：

- 一次性的簡單翻譯或潤稿。
- 不需要來源、證據或方法判斷的短任務。
- 基礎模型在固定測試中已經和 Skill 一樣穩定。

Skill 的主要價值是穩定程序，不是增加模型智力。重要任務建議明確呼叫 Skill，不要只依賴自動觸發。

## 核心原則

- 不以「AI 味」判定作者身分或違規。
- 不提供沒有有效依據的 AI 生成機率。
- 搜尋不到只代表尚未核實。
- 存在性、書目資訊、主張支持、品質與適用性分開判斷。
- 依學科、方法、研究階段與當前規定調整標準。
- 多代理人一致不當然構成獨立驗證。
- 未經要求不代寫或重寫全文。
- 關鍵來源仍須由作者閱讀原文。

## 測試而不是相信

請依 [TESTING.md](./TESTING.md) 在相同平台與模型比較：

- A：不使用 Skill。
- B：上傳 Lite 版。
- C：使用原生 Skill。

比較觸發、正確性、相對基準的實際增量，以及是否增加不必要的文字、步驟、時間與工具呼叫。原生 Skill 另附十組 `evals/evals.json` 測試。

## 工具限制

- 沒有網頁搜尋，就不能完成即時來源查證。
- 沒有資料庫或全文權限，就不能宣稱已核對付費全文。
- 只能看到摘要時，不能把摘要層級的查核寫成全文驗證。
- `extract_citation_ids.py` 只能擷取 DOI、ISBN 與 URL 候選，不能證明來源存在或支持主張。

## 隱私與研究倫理

上傳未公開論文、訪談稿、個資、同儕審查資料或授權全文前，請先確認機構、課程、期刊及研究倫理規範。必要時去識別化、刪除非必要資料，或改用經核准的環境。

## 分享時可使用的說明

> 下載 Lite MD，上傳到支援附件的 AI，就能使用一套作者自我檢查程序；不需要先安裝。它不是 AI 偵測器，而是協助作者區分寫作問題、來源狀態、證據限制、方法適切性與 AI 輔助研究責任。
