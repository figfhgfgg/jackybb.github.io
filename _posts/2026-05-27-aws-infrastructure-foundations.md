---
layout: default
title: "AWS 雲端基礎設施與自動化部署維運筆記"
date: 2026-05-27
---

# ☁️ AWS 雲端基礎設施與自動化部署維運筆記

本篇筆記記錄了企業級雲端架構的底層維運核心，涵蓋無伺服器部署（Serverless）、網路控制、權限邊界、以及多 AI 模型整合的實戰思維。

---

## 🏗️ 第一部分：無伺服器 AI 應用部署架構 (Serverless AI Deployment)

現代雲端維運（DevOps/SecOps）的核心目標，是確保權限邊界明確與系統安全運維，並從傳統的手動組態逐步演進為「基礎設施即代碼（IaC）」與自動化部署。

### 1. 開發端與託管流程 (Development Flow)
* **原始碼與 CI/CD 管道**：透過 `GitHub` 進行程式碼版本管理，並利用 `GitHub Actions` 建立自動化 CI/CD 流水線。
* **容器化與基礎設施定義**：
  * 使用 `Dockerfile` 定義運行環境，將服務與軟體映像檔進行封裝建置。
  * 透過 AWS `CDK (Cloud Development Kit)` 以程式碼定義雲端基礎設施（IaC），取代手動點擊主控台。
* **Serverless 雲端代管**：將建置好的容器服務映像檔或網頁靜態檔案，部署至 AWS 的伺服器定義與基礎設施定義中（如 `EC2` 計算資源與 `S3` 儲存貯體），實現全代管的彈性擴展。

### 2. 安全與合規機制 (Security & Compliance)
* **權限控制 (Authorization)**：嚴格遵循最小權限原則。
* **Role (IAM 角色)**：為不同的雲端元件與服務劃分明確的邊界。
* **跨平台擴展**：此套設計邏輯與控制機制不僅適用於 AWS，亦可橫向無縫遷移至 Azure、GCP、Oracle 等多雲架構。

---

## 🌐 第二部分：高可用性網路拓撲與路由控制

為了支撐高效能與自動化擴展（Auto Scaling）的 AI 應用工作負載，網路架構必須具備強大的路由控制與安全性。

### 1. 網路流量引導鏈 (Ingress Path)
當外部使用者存取內部服務時，標準的網路傳輸鏈路如下：
`ZGW (區域網關) ➡️ 自訂網域 (Domain) ➡️ 路由表 (Route Table) ➡️ 網際網路閘道器 (IGW) ➡️ 安全群組 (SG) ➡️ 運算節點 (EC2)`

### 2. 網路架構演進：V1 基礎版 ➡️ V2 AI 集成版
* **網路架構 V1（基礎安全性）**：
  * 在 VPC 內切分出**公有子網路 (Public Subnet)** 與**私有子網路 (Private Subnet)**。
  * 公有網段透過 `IGW` 連接外網，並依賴 `Route Table` 控制基礎路由。
* **網路架構 V2（AI 整合與 Route Table 2.0）**：
  * **進階路由控制**：升級為 `Route Table 2.0`，將網關（`ZGW`）與 `IGW` 的路由規則深度整合，加強子網安全性。
  * **高性能 AI 模型代理 (AI Model Agent)**：私有網段內的運算節點（如後端服務）透過加密安全的子網管道，向外部主流的高性能 AI 模型（如 OpenAI `GPT-4`、Anthropic `Claude 3 Opus`、Google `Gemini 1.5 Pro`）發起連線與權限映射，完成多 AI 協同工作。

---

## 🤖 第三部分：高智慧 AI-Agent 應用架構落地

企業在落地 AI 應用時，必須透過「雙擔保約織」來確保內容的可信度與流程的精準度。

### 1. 記憶、外掛與能力映射
* **Memory (記憶機制)**：區分為快取（高速暫存）、長短期記憶以及外部存儲系統，藉此提高 AI 的反應快取率。
* **Plugin (外掛功能)**：將 AI 代理（AI-Agent）連接至第三方服務 API，將雲端平台（AWS/GCP/Azure）的能力（Skill）直接映射給 AI，使其具備實際動手操作軟體的能力。

### 2. 提示詞工程與流程控制 (Prompting Flow)
* **Prompt Engineering**：利用少樣本提示（Few-shot / Zero-shot）、鏈式思考（Chain of thought）與嚴格的模板（Template）來引導 LLM，確保內容的可信度。
* **Red Team 攻擊模擬（紅字警示）**：系統必須具備防禦提示注入（Prompt Injection）、提示詞洩漏（Prompt Leakage）與惡意提示攻擊（Prompt Attack）的安全防禦機制。
* **Flow Control (流程控制網關)**：
  * AI 模型產出初步回應後，不直接交付前端。
  * 透過流程控制節點，讓多個 AI 進行**「問題分析 ➡️ 參數/自動提核 ➡️ 高級校正」**的協同工作，確保最終輸出符合企業生產環境的高等標準。
