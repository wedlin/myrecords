**在 VS Code 裡可以直接做到邊寫 Markdown 邊看效果**
操作方式：
打開你的 .md 檔。
按下 快捷鍵：
Windows / Linux：Ctrl + K V
macOS：Cmd + K V
（先按 Ctrl + K，再放開，再按 V）
這樣 VS Code 就會自動 分割畫面：左邊編輯 Markdown、右邊即時顯示排版效果。

---

**中文亂碼問題 / 正確的轉檔方式**
方法 1：加 BOM 的 UTF-8（推薦 Windows）
確保檔案 Reopen with Encoding → Big5 正確顯示。
點右下角編碼 → Save with Encoding → UTF-8 with BOM。
存檔後重新打開，中文就不會亂碼。
BOM（Byte Order Mark）是 Windows 很常用的「檔頭標記」，能幫系統辨識 UTF-8 中文檔。

---
