# 新增測試案例說明報告（Backend）

## 1. 測試目標
本次在後端 Todo API 新增 2 個測試案例，補強「建立資料」與「刪除資料」兩條重要流程，確保 API 行為符合規格。

## 2. 測試工具
- 測試框架：Vitest
- HTTP 測試方式：Fastify 的 `server.inject()`（不需實際啟動網路埠）
- Mock 工具：Vitest `vi.spyOn()`

## 3. 新增測試案例

### Case A：建立 Todo 成功
- 測試檔案位置：`backend/test/todo.spec.ts`
- 測試名稱：
  - Given a valid todo payload, When receive a POST /api/v1/todos request, Then it should response the created todo with status code 201
- 測試主體：`POST /api/v1/todos`
- 測試場景：送出合法 payload（name + description）
- Mock 策略：
  - 將 `createTodo` mock 成回傳一筆建立完成的 todo 物件
- 預期結果：
  - HTTP status 為 201
  - response body 的 `todo` 與 mock 資料完全一致
- 想驗證的重點：
  - 路由層有正確呼叫建立流程
  - API 回傳格式與狀態碼符合 REST 建立資源語意

### Case B：刪除 Todo 成功
- 測試檔案位置：`backend/test/todo.spec.ts`
- 測試名稱：
  - Given a valid ID, When receive a DELETE /api/v1/todos/:id request, Then it should response with status code 204
- 測試主體：`DELETE /api/v1/todos/:id`
- 測試場景：傳入存在的 id 進行刪除
- Mock 策略：
  - 將 `deleteTodoById` mock 成回傳成功刪除結果（含 `deletedCount: 1`）
- 預期結果：
  - HTTP status 為 204
  - response body 為空字串（No Content）
- 想驗證的重點：
  - 刪除流程成功時，API 是否遵守 204 No Content 規格

## 4. 測試策略說明
本次策略以「路由層行為測試」為主：
1. 隔離資料庫：透過 mock repo 函式，不依賴真實 DB 狀態。
2. 驗證 API 合約：聚焦狀態碼與回傳 body 格式。
3. 補齊核心 CRUD 情境：既有測試已覆蓋讀取與更新，本次補上建立與刪除成功路徑。

## 5. 執行結果
在 backend 目錄執行測試指令：
- `npm run test`

結果：
- Test Files：3 passed
- Tests：12 passed
- 新增的 2 個 test case 全數通過
