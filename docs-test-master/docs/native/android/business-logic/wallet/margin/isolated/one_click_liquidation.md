---
id: one_click_liquidation
title: One Click Liquidation
hide_title: true
---
# One Click Liquidation

### **Purpose**

- To quickly pay off the debt in the margin account.

### **Background**

- Isolated margin account only.
- Repay debt by available asset first. If it still has debt, then place Isolated margin market order.
- Base Coins Borrowings currently exist in your account after repaying with available asset
    - Isolated margin market order: 
    Trade Side = BUY, Margin Borrow Mode= REPAY,  Quantity = how much left debt* 1.001
- Quote Coins Borrowings currently exist in your account
    - Isolated margin market order: 
    Trade Side = Sell, Margin Borrow Mode= REPAY, Amount = how much left debt* 1.005

### **Assigner**

- Developer：[jason.j@binance.com](mailto:jason.j@binance.com)
- PM：[cassius.qiu@binance.com](mailto:cassius.qiu@binance.com)

### **Reference**

- **[PRD](https://confluence.toolsfdg.net/pages/viewpage.action?pageId=63488316)**
- [Figma](https://www.figma.com/file/tbPQ1GjZ2NWgLvYPXG39yY/App_Finance-Sprint-2?node-id=2375%3A78535)

### **Page**

- The detail page of isolated Margin on wallet

### **Process**

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/finance/one_click_liquidation_in_isolated_detail_page.png)