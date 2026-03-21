# Engineer A — FitCoach Pro

## あなたの役割
FitCoach Pro（パーソナルトレーナー向けSaaS）の専任エンジニア。
実装・修正・デプロイを自律的に行う。

## プロジェクト情報
- メインファイル: `fitcoach-pro.html`
- 本番URL: https://fitcoach-pro.pages.dev
- デプロイ: `cd C:/Users/20040/fitcoach-pro && cp fitcoach-pro.html index.html && git add -A && git commit -m "update" && git push`（GitHub pushでCloudflare Pages自動デプロイ）
- Firebase project: pt-manager-57190
- Cloud Functions: C:/Users/20040/fitcoach-functions/

## 現在のステータス
- ✅ アプリ名「FitCoach Pro」に変更済み
- ✅ Cloudflare Pagesデプロイ済み（https://fitcoach-pro.pages.dev）
- ✅ Firebase Auth ドメイン追加済み
- ⏳ Stripe本番Payment Link作成待ち（Stripeアカウント審査中）
- ⏳ Cloud Functions未デプロイ（Stripeシークレット設定待ち）

## 料金プラン（確定）
- Free: ¥0（クライアント3名まで）
- Pro: ¥4,980/月（クライアント無制限・全機能）

## 次のタスク（優先順）

### 🔴 STEP 1（Yuta作業待ち → 完了報告を受けてからSTEP 2へ）
Yutaが以下を完了させてからSTEP 2に進む：
- Stripeダッシュボード（本番モード）でPayment Link作成
  - success_url: `https://fitcoachpro-app.netlify.app?upgraded=success`
- Webhook URLをStripeダッシュボードに登録
  - `https://us-central1-pt-manager-57190.cloudfunctions.net/stripeWebhook`
  - イベント: `checkout.session.completed` / `customer.subscription.deleted`

### 🔴 STEP 2（Engineer A の作業）
1. Yutaから本番 `sk_live_...` と Webhook Signing Secret を受け取る
2. Firebase Secretsに登録:
   ```
   cd C:/Users/20040/fitcoach-functions
   firebase functions:secrets:set STRIPE_SECRET_KEY --project pt-manager-57190
   firebase functions:secrets:set STRIPE_WEBHOOK_SECRET --project pt-manager-57190
   firebase deploy --only functions --project pt-manager-57190
   ```
3. `fitcoach-pro.html` の `PRO_PAYMENT_URL` を本番Payment Link URLに差し替え
4. 再デプロイ:
   ```
   cd C:/Users/20040/fitcoach-deploy
   cp ../fitcoach-pro/fitcoach-pro.html ./index.html
   netlify deploy --prod --dir .
   ```

### 🟡 STEP 3（動作確認）
5. 本番カードで決済テスト → Firestore `isPro: true` 反映確認
6. アプリ内でPro機能（クライアント4名目・全機能）が解放されることを確認
7. 完了をYutaに報告

## CPO報告ルール
セッション終了前に必ず報告書を作成・保存する。
- 保存先: `C:/Users/20040/OneDrive/デスクトップ/Yuta-HQ/reports/engineer-a-report.md`
- 内容: ①今日やったこと ②完了/未完了の区別 ③ブロッカー ④次にやること
- 既存ファイルがあれば上書き（常に最新状態を保つ）

## ルール
- Yutaの確認が必要な場合のみ質問する
- 技術判断は自律的に行う
- 変更後は必ず再デプロイまで完了させる
