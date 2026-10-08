---
title: 選擇題測驗卷網站講義（學生版）.md

---

---
title: 選擇題測驗卷網站講義（學生版）

---

---
title: 選擇題測驗卷網站講義（學生版）
tags: [114程式設計與實習_上學期]

---

# 選擇題測驗卷網站講義（學生版）

學號：＿＿＿＿＿＿＿＿　　姓名：＿＿＿＿＿＿＿＿

> **填寫方式**
> 1. 每個學習都要放：**執行截圖**、**三次問 AI 的提示詞**、**最後採用的程式碼**。
> 2. 問 AI 的提示詞請**逐字貼上**自己實際輸入的內容（不要寫摘要），第一次、第二次、第三次依序記錄。
> 3. 程式碼貼在「點開貼上」的收合區塊裡，貼上**你最後真正採用、而且能執行**的版本。

---

## 學習1：產生一個選擇題測驗卷網站

https://cfchen58.synology.me/115/week4/stage1/

**這個階段的目標：** 用 p5.js 做出一個一次顯示一題、四個選項、答完會顯示對錯與總分的測驗網站（題目先寫在程式裡）。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）
![螢幕擷取畫面 2026-10-08 142937](https://hackmd.io/_uploads/SJWRd2NiMx.png)
![螢幕擷取畫面 2026-10-08 143204](https://hackmd.io/_uploads/SJBeY34sMx.png)
![螢幕擷取畫面 2026-10-08 143226](https://hackmd.io/_uploads/Syo-K3VoGe.png)



### 第一次問 AI

```tex!
使用p5.js攥寫一個選擇題網頁測驗系統，我已經產生一個p5.js專案，請把程式碼寫到sketch.js檔案內，每條指令都需要加上中文註解。測驗系統題目設定為五題，測驗題目的內容為程式設計p5.js簡易指令練習測驗，系統採用全螢幕畫布，使用者答錯時，系統會在正確答案選項上，加上e0fbfc背景顏色，該選項要上下跳動，答錯的選項採用ff758f背景顏色，選項左右移動，選擇題選項共有四個，當五題結束之後，需要顯示答對的題數，每次顯示一個題目，需要有下一題的按鈕
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習1的程式碼
```javascript=
// 宣告目前所在的測驗題目索引。
let currentQuestion = 0; // 將第一題設定為目前題目。
let score = 0; // 儲存使用者目前答對的題數。
let selectedIndex = -1; // 記錄目前選取的選項索引。
let hasAnswered = false; // 記錄目前題目是否已經作答。
let quizFinished = false; // 記錄五題是否已經全部完成。
let lastInputTime = 0; // 記錄最近一次輸入時間以避免觸控重複觸發。

// 建立五題 p5.js 基礎指令選擇題資料。
const questions = [ // 宣告所有測驗題目的陣列。
  { // 建立第一題的物件資料。
    prompt: "哪一個函式可以在 p5.js 中建立畫布？", // 設定第一題題目文字。
    options: ["createCanvas()", "makeCanvas()", "newCanvas()", "canvas()"], // 設定第一題四個選項。
    answer: 0 // 設定第一題的正確選項索引。
  }, // 結束第一題物件。
  { // 建立第二題的物件資料。
    prompt: "哪一個函式會在 p5.js 程式開始時執行一次？", // 設定第二題題目文字。
    options: ["start()", "setup()", "begin()", "init()"], // 設定第二題四個選項。
    answer: 1 // 設定第二題的正確選項索引。
  }, // 結束第二題物件。
  { // 建立第三題的物件資料。
    prompt: "哪一個函式通常會在 p5.js 中持續重複執行？", // 設定第三題題目文字。
    options: ["loopOnce()", "repeat()", "draw()", "run()"], // 設定第三題四個選項。
    answer: 2 // 設定第三題的正確選項索引。
  }, // 結束第三題物件。
  { // 建立第四題的物件資料。
    prompt: "哪一個函式可以在畫布上畫出圓形？", // 設定第四題題目文字。
    options: ["circle()", "ellipse()", "round()", "oval()"], // 設定第四題四個選項。
    answer: 1 // 設定第四題的正確選項索引。
  }, // 結束第四題物件。
  { // 建立第五題的物件資料。
    prompt: "哪一個函式可以設定圖形的填滿顏色？", // 設定第五題題目文字。
    options: ["paint()", "background()", "fill()", "colorFill()"], // 設定第五題四個選項。
    answer: 2 // 設定第五題的正確選項索引。
  } // 結束第五題物件。
]; // 結束測驗題目陣列。

// p5.js 在程式開始時會自動呼叫此函式。
function setup() { // 定義初始化函式。
  createCanvas(windowWidth, windowHeight); // 建立符合視窗大小的全螢幕畫布。
  pixelDensity(Math.min(window.devicePixelRatio || 1, 2)); // 限制像素密度以兼顧清晰度與效能。
  textFont("sans-serif"); // 設定不依賴外部素材的系統字型。
  textAlign(CENTER, CENTER); // 將文字預設設定為水平與垂直置中。
  rectMode(CORNER); // 將矩形繪製模式設定為左上角定位。
  noStroke(); // 預設關閉圖形外框線。
  describe("p5.js 基礎指令五題選擇題測驗介面"); // 提供畫布的無障礙文字描述。
} // 結束初始化函式。

// p5.js 會持續呼叫此函式來繪製畫面。
function draw() { // 定義每一幀的繪圖函式。
  drawBackground(); // 繪製適應全螢幕的背景。
  if (quizFinished) { // 判斷是否已經完成全部題目。
    drawResultScreen(); // 完成測驗時繪製成績畫面。
  } else { // 處理尚未完成測驗的情況。
    drawQuizScreen(); // 尚未完成時繪製目前題目的畫面。
  } // 結束完成狀態判斷。
} // 結束每一幀的繪圖函式。

// 繪製不依賴外部圖片的柔和背景。
function drawBackground() { // 定義背景繪製函式。
  background("#f7f9fc"); // 使用淺色背景清除上一幀畫面。
  noStroke(); // 確保背景裝飾不顯示外框線。
  fill("#e8f1f5"); // 設定左上方裝飾圓的顏色。
  circle(width * 0.08, height * 0.08, Math.min(width, height) * 0.22); // 繪製左上方裝飾圓。
  fill("#eef3ff"); // 設定右下方裝飾圓的顏色。
  circle(width * 0.93, height * 0.91, Math.min(width, height) * 0.3); // 繪製右下方裝飾圓。
} // 結束背景繪製函式。

// 取得依照視窗尺寸計算的版面資訊。
function getLayout() { // 定義響應式版面計算函式。
  const narrow = width < 640; // 判斷目前是否為窄螢幕版面。
  const margin = constrain(width * 0.07, 18, 72); // 計算左右內容邊界距離。
  const contentWidth = width - margin * 2; // 計算主要內容可使用寬度。
  const headerY = constrain(height * 0.09, 52, 88); // 計算標題垂直位置。
  const cardY = narrow ? height * 0.17 : height * 0.19; // 計算題目卡片的垂直位置。
  const cardHeight = narrow ? height * 0.22 : height * 0.18; // 計算題目卡片高度。
  const optionGap = narrow ? 12 : 18; // 計算選項之間的間距。
  const columns = narrow ? 1 : 2; // 窄螢幕使用單欄，寬螢幕使用雙欄。
  const optionWidth = columns === 1 ? contentWidth : (contentWidth - optionGap) / 2; // 計算每個選項寬度。
  const optionHeight = narrow ? constrain(height * 0.075, 54, 70) : constrain(height * 0.105, 64, 86); // 計算每個選項高度。
  const optionStartY = cardY + cardHeight + (narrow ? 20 : 28); // 計算第一個選項的垂直位置。
  const buttonWidth = constrain(contentWidth * 0.42, 150, 260); // 計算下一題按鈕寬度。
  const buttonHeight = constrain(height * 0.075, 54, 68); // 計算下一題按鈕高度。
  const buttonY = height - margin - buttonHeight; // 計算下一題按鈕垂直位置。
  return { margin, contentWidth, headerY, cardY, cardHeight, optionGap, columns, optionWidth, optionHeight, optionStartY, buttonWidth, buttonHeight, buttonY }; // 回傳完整響應式版面資訊。
} // 結束版面計算函式。

// 取得指定選項在目前視窗中的矩形範圍。
function getOptionRect(index) { // 定義選項位置計算函式。
  const layout = getLayout(); // 取得目前視窗的版面資訊。
  const column = index % layout.columns; // 計算選項所在的欄位。
  const row = Math.floor(index / layout.columns); // 計算選項所在的列數。
  const x = layout.margin + column * (layout.optionWidth + layout.optionGap); // 計算選項左側座標。
  const y = layout.optionStartY + row * (layout.optionHeight + layout.optionGap); // 計算選項上方座標。
  return { x, y, w: layout.optionWidth, h: layout.optionHeight }; // 回傳選項矩形資料。
} // 結束選項位置計算函式。

// 繪製目前的測驗題目畫面。
function drawQuizScreen() { // 定義測驗畫面繪製函式。
  const layout = getLayout(); // 取得目前視窗的版面資訊。
  const question = questions[currentQuestion]; // 取得目前題目的資料。
  const baseSize = constrain(Math.min(width, height) * 0.04, 16, 28); // 計算適合視窗的基準字體大小。
  fill("#183642"); // 設定主標題文字顏色。
  textStyle(BOLD); // 將主標題設定為粗體。
  textSize(constrain(baseSize * 1.3, 22, 36)); // 設定主標題文字大小。
  text("p5.js 指令小測驗", width / 2, layout.headerY); // 顯示測驗主標題。
  textStyle(NORMAL); // 將後續文字恢復為一般字體。
  fill("#52727b"); // 設定進度文字顏色。
  textSize(constrain(baseSize * 0.65, 13, 18)); // 設定進度文字大小。
  text(`第 ${currentQuestion + 1} 題／共 ${questions.length} 題`, width / 2, layout.headerY + baseSize * 1.4); // 顯示目前答題進度。
  drawQuestionCard(question, layout, baseSize); // 繪製目前題目的文字卡片。
  for (let index = 0; index < question.options.length; index += 1) { // 逐一繪製四個選項。
    drawOption(question, index, baseSize); // 繪製指定索引的選項。
  } // 結束四個選項的繪製迴圈。
  if (hasAnswered) { // 判斷使用者是否已經選擇答案。
    drawFeedback(question, layout, baseSize); // 顯示作答結果與提示文字。
    drawNextButton(layout, baseSize); // 顯示作答後才能使用的下一題按鈕。
  } // 結束作答狀態判斷。
} // 結束測驗畫面繪製函式。

// 繪製題目文字卡片。
function drawQuestionCard(question, layout, baseSize) { // 定義題目卡片繪製函式。
  fill("#ffffff"); // 設定題目卡片填滿色。
  rect(layout.margin, layout.cardY, layout.contentWidth, layout.cardHeight, 20); // 繪製圓角題目卡片。
  fill("#183642"); // 設定題目文字顏色。
  textStyle(BOLD); // 將題目文字設定為粗體。
  textSize(constrain(baseSize * 0.92, 18, 27)); // 設定題目文字大小。
  drawWrappedText(question.prompt, width / 2, layout.cardY + layout.cardHeight / 2, layout.contentWidth - 36, baseSize * 1.3); // 以自動換行方式繪製題目。
  textStyle(NORMAL); // 將文字樣式恢復為一般。
} // 結束題目卡片繪製函式。

// 繪製單一選項並套用作答動畫與顏色。
function drawOption(question, index, baseSize) { // 定義選項繪製函式。
  const rectData = getOptionRect(index); // 取得指定選項的原始矩形位置。
  let offsetX = 0; // 初始化選項的水平動畫位移。
  let offsetY = 0; // 初始化選項的垂直動畫位移。
  const isCorrect = index === question.answer; // 判斷目前選項是否為正確答案。
  const isSelected = index === selectedIndex; // 判斷目前選項是否為使用者選取答案。
  if (hasAnswered && !isCorrect && isSelected) { // 判斷是否為答錯且被選取的選項。
    offsetX = Math.sin(frameCount * 0.35) * 9; // 讓答錯選項持續左右移動。
  } // 結束答錯選項動畫判斷。
  if (hasAnswered && !isCorrect && selectedIndex !== question.answer && isCorrect) { // 保留正確答案動畫判斷的結構。
    offsetY = 0; // 讓不符合條件的分支維持零位移。
  } // 結束不會觸發的相容判斷。
  if (hasAnswered && selectedIndex !== question.answer && isCorrect) { // 判斷答錯後的正確答案選項。
    offsetY = Math.sin(frameCount * 0.32) * 9; // 讓正確答案選項持續上下跳動。
  } // 結束正確答案動畫判斷。
  push(); // 保存目前的繪圖樣式與座標狀態。
  translate(offsetX, offsetY); // 套用選項的動畫位移。
  if (!hasAnswered) { // 判斷尚未作答的狀態。
    fill("#ffffff"); // 將未作答選項設定為白色背景。
  } else if (isCorrect && selectedIndex !== question.answer) { // 判斷答錯後的正確選項。
    fill("#e0fbfc"); // 將正確答案背景設定為指定的淺藍色。
  } else if (isSelected && !isCorrect) { // 判斷使用者答錯的選項。
    fill("#ff758f"); // 將答錯選項背景設定為指定的粉紅色。
  } else if (isSelected && isCorrect) { // 判斷使用者答對並選中正確選項的狀態。
    fill("#b7efc5"); // 將答對的選項設定為明確的綠色背景。
  } else { // 處理作答後未被選取的其他選項。
    fill("#ffffff"); // 將其他選項維持為白色背景。
  } // 結束選項背景顏色判斷。
  rect(rectData.x, rectData.y, rectData.w, rectData.h, 16); // 繪製圓角選項按鈕。
  if (isSelected) { // 判斷目前選項是否被使用者選取。
    stroke("#183642"); // 為選取的選項加上深色外框。
    strokeWeight(3); // 設定選取外框的線條寬度。
    noFill(); // 設定外框繪製時不填滿內部。
    rect(rectData.x, rectData.y, rectData.w, rectData.h, 16); // 繪製選取選項的外框。
    noStroke(); // 恢復關閉外框線以免影響下一個圖形。
  } // 結束選取外框判斷。
  fill("#183642"); // 設定選項文字顏色。
  textSize(constrain(baseSize * 0.72, 15, 22)); // 設定選項文字大小。
  drawWrappedText(`${String.fromCharCode(65 + index)}. ${question.options[index]}`, rectData.x + rectData.w / 2, rectData.y + rectData.h / 2, rectData.w - 28, baseSize); // 繪製選項編號與文字。
  pop(); // 還原繪圖樣式與座標狀態。
} // 結束單一選項繪製函式。

// 繪製作答後的明確回饋文字。
function drawFeedback(question, layout, baseSize) { // 定義作答回饋繪製函式。
  const correct = selectedIndex === question.answer; // 判斷本題是否答對。
  const feedbackY = layout.buttonY - baseSize * 0.9; // 計算回饋文字的垂直位置。
  fill(correct ? "#237a57" : "#b4233d"); // 依照答題結果設定回饋文字顏色。
  textStyle(BOLD); // 將回饋文字設定為粗體。
  textSize(constrain(baseSize * 0.75, 15, 21)); // 設定回饋文字大小。
  if (correct) { // 判斷答對時的回饋內容。
    text("答對了！你選擇的答案正確。", width / 2, feedbackY); // 明確顯示答對結果。
  } else { // 處理答錯時的回饋內容。
    text(`答錯了！正確答案是 ${String.fromCharCode(65 + question.answer)}。`, width / 2, feedbackY); // 明確顯示答錯與正確答案。
  } // 結束答題回饋判斷。
  textStyle(NORMAL); // 將文字樣式恢復為一般。
} // 結束作答回饋繪製函式。

// 繪製作答後出現的下一題按鈕。
function drawNextButton(layout, baseSize) { // 定義下一題按鈕繪製函式。
  const buttonX = width / 2 - layout.buttonWidth / 2; // 計算下一題按鈕左側座標。
  fill("#247ba0"); // 設定下一題按鈕背景顏色。
  rect(buttonX, layout.buttonY, layout.buttonWidth, layout.buttonHeight, 16); // 繪製圓角下一題按鈕。
  fill("#ffffff"); // 設定下一題按鈕文字顏色。
  textStyle(BOLD); // 將按鈕文字設定為粗體。
  textSize(constrain(baseSize * 0.78, 16, 22)); // 設定按鈕文字大小。
  text(currentQuestion === questions.length - 1 ? "查看結果" : "下一題", width / 2, layout.buttonY + layout.buttonHeight / 2); // 顯示下一題或查看結果文字。
  textStyle(NORMAL); // 將文字樣式恢復為一般。
} // 結束下一題按鈕繪製函式。

// 繪製五題完成後的成績畫面。
function drawResultScreen() { // 定義結果畫面繪製函式。
  const baseSize = constrain(Math.min(width, height) * 0.045, 18, 32); // 計算結果畫面的基準字體大小。
  const cardWidth = Math.min(width * 0.86, 620); // 計算結果卡片寬度。
  const cardHeight = Math.min(height * 0.58, 430); // 計算結果卡片高度。
  const cardX = width / 2 - cardWidth / 2; // 計算結果卡片左側座標。
  const cardY = height / 2 - cardHeight / 2; // 計算結果卡片上方座標。
  fill("#ffffff"); // 設定結果卡片背景顏色。
  rect(cardX, cardY, cardWidth, cardHeight, 24); // 繪製圓角結果卡片。
  fill("#183642"); // 設定結果標題文字顏色。
  textStyle(BOLD); // 將結果標題設定為粗體。
  textSize(constrain(baseSize * 1.35, 26, 42)); // 設定結果標題文字大小。
  text("測驗完成！", width / 2, cardY + cardHeight * 0.24); // 顯示測驗完成標題。
  fill("#247ba0"); // 設定分數文字顏色。
  textSize(constrain(baseSize * 1.8, 40, 70)); // 設定分數文字大小。
  text(`${score}／${questions.length} 題答對`, width / 2, cardY + cardHeight * 0.48); // 顯示答對題數。
  fill("#52727b"); // 設定鼓勵文字顏色。
  textSize(constrain(baseSize * 0.7, 15, 21)); // 設定鼓勵文字大小。
  text(score === questions.length ? "太棒了！全部答對！" : "再練習一次，你會越來越熟悉！", width / 2, cardY + cardHeight * 0.64); // 顯示依分數變化的鼓勵文字。
  const buttonWidth = constrain(cardWidth * 0.5, 170, 260); // 計算重新開始按鈕寬度。
  const buttonHeight = constrain(height * 0.075, 54, 68); // 計算重新開始按鈕高度。
  const buttonX = width / 2 - buttonWidth / 2; // 計算重新開始按鈕左側座標。
  const buttonY = cardY + cardHeight * 0.76; // 計算重新開始按鈕上方座標。
  fill("#247ba0"); // 設定重新開始按鈕背景顏色。
  rect(buttonX, buttonY, buttonWidth, buttonHeight, 16); // 繪製圓角重新開始按鈕。
  fill("#ffffff"); // 設定重新開始按鈕文字顏色。
  textSize(constrain(baseSize * 0.78, 16, 22)); // 設定重新開始按鈕文字大小。
  text("重新開始", width / 2, buttonY + buttonHeight / 2); // 顯示重新開始按鈕文字。
  textStyle(NORMAL); // 將文字樣式恢復為一般。
} // 結束結果畫面繪製函式。

// 將沒有空格的中文題目或選項依照寬度切成多行。
function wrapText(message, maxWidth) { // 定義文字換行函式。
  const lines = []; // 建立儲存每一行文字的陣列。
  let currentLine = ""; // 建立目前正在組合的文字行。
  for (const character of message) { // 逐字檢查文字是否超過可用寬度。
    const testLine = currentLine + character; // 暫時組合加入下一個字後的文字行。
    if (textWidth(testLine) > maxWidth && currentLine.length > 0) { // 判斷加入新字後是否超過寬度。
      lines.push(currentLine); // 將已經完成的文字行存入陣列。
      currentLine = character; // 以目前字元開始下一行。
    } else { // 處理尚未超過寬度的情況。
      currentLine = testLine; // 將暫時文字行設定為目前文字行。
    } // 結束文字寬度判斷。
  } // 結束逐字檢查迴圈。
  if (currentLine.length > 0) { // 判斷是否還有最後一行尚未存入。
    lines.push(currentLine); // 將最後一行文字存入陣列。
  } // 結束最後一行判斷。
  return lines; // 回傳換行後的文字陣列。
} // 結束文字換行函式。

// 依照行高繪製已經換行的文字。
function drawWrappedText(message, centerX, centerY, maxWidth, lineHeight) { // 定義多行文字繪製函式。
  const lines = wrapText(message, maxWidth); // 取得依寬度切分後的文字行。
  const totalHeight = (lines.length - 1) * lineHeight; // 計算全部文字行的總高度。
  for (let index = 0; index < lines.length; index += 1) { // 逐行繪製換行文字。
    const lineY = centerY - totalHeight / 2 + index * lineHeight; // 計算目前文字行的垂直位置。
    text(lines[index], centerX, lineY); // 繪製目前文字行。
  } // 結束逐行繪製迴圈。
} // 結束多行文字繪製函式。

// 判斷座標是否位於指定矩形內。
function isInside(pointX, pointY, rectData) { // 定義矩形碰撞判斷函式。
  return pointX >= rectData.x && pointX <= rectData.x + rectData.w && pointY >= rectData.y && pointY <= rectData.y + rectData.h; // 回傳座標是否位於矩形內。
} // 結束矩形碰撞判斷函式。

// 處理滑鼠與觸控共用的選取座標。
function handlePointer(pointX, pointY) { // 定義滑鼠與觸控共用的輸入處理函式。
  if (millis() - lastInputTime < 250) { // 判斷是否在短時間內重複觸發輸入。
    return; // 忽略過短時間內的重複輸入。
  } // 結束重複輸入判斷。
  lastInputTime = millis(); // 更新最近一次輸入時間。
  if (quizFinished) { // 判斷目前是否位於結果畫面。
    handleRestartPointer(pointX, pointY); // 將輸入交由重新開始按鈕處理。
    return; // 結束結果畫面的輸入處理。
  } // 結束結果畫面判斷。
  if (!hasAnswered) { // 判斷目前題目是否尚未作答。
    handleOptionPointer(pointX, pointY); // 將輸入交由四個選項處理。
  } else { // 處理已經完成作答的狀態。
    handleNextPointer(pointX, pointY); // 將輸入交由下一題按鈕處理。
  } // 結束作答狀態判斷。
} // 結束共用輸入處理函式。

// 處理尚未作答時的選項點擊或觸控。
function handleOptionPointer(pointX, pointY) { // 定義選項輸入處理函式。
  const question = questions[currentQuestion]; // 取得目前題目的資料。
  for (let index = 0; index < question.options.length; index += 1) { // 逐一檢查四個選項的碰撞範圍。
    const rectData = getOptionRect(index); // 取得目前選項的矩形範圍。
    if (isInside(pointX, pointY, rectData)) { // 判斷輸入座標是否落在目前選項內。
      selectedIndex = index; // 記錄使用者所選的選項索引。
      hasAnswered = true; // 鎖定本題答案並顯示回饋。
      if (selectedIndex === question.answer) { // 判斷使用者是否答對本題。
        score += 1; // 將答對題數增加一題。
      } // 結束答對分數判斷。
      return; // 選到選項後結束檢查迴圈。
    } // 結束選項碰撞判斷。
  } // 結束檢查四個選項的迴圈。
} // 結束選項輸入處理函式。

// 處理下一題或查看結果按鈕的點擊與觸控。
function handleNextPointer(pointX, pointY) { // 定義下一題按鈕輸入處理函式。
  const layout = getLayout(); // 取得目前視窗的版面資訊。
  const buttonRect = { x: width / 2 - layout.buttonWidth / 2, y: layout.buttonY, w: layout.buttonWidth, h: layout.buttonHeight }; // 建立下一題按鈕的碰撞矩形。
  if (isInside(pointX, pointY, buttonRect)) { // 判斷輸入座標是否位於下一題按鈕內。
    if (currentQuestion < questions.length - 1) { // 判斷是否還有下一題可以繼續。
      currentQuestion += 1; // 將題目索引移至下一題。
      selectedIndex = -1; // 清除下一題的選取狀態。
      hasAnswered = false; // 讓下一題回到尚未作答狀態。
    } else { // 處理已經答完第五題的情況。
      quizFinished = true; // 切換至測驗結果畫面。
    } // 結束題目進度判斷。
  } // 結束下一題按鈕碰撞判斷。
} // 結束下一題按鈕輸入處理函式。

// 處理結果畫面重新開始按鈕的點擊與觸控。
function handleRestartPointer(pointX, pointY) { // 定義重新開始按鈕輸入處理函式。
  const cardWidth = Math.min(width * 0.86, 620); // 計算結果卡片寬度。
  const cardHeight = Math.min(height * 0.58, 430); // 計算結果卡片高度。
  const cardY = height / 2 - cardHeight / 2; // 計算結果卡片上方座標。
  const buttonWidth = constrain(cardWidth * 0.5, 170, 260); // 計算重新開始按鈕寬度。
  const buttonHeight = constrain(height * 0.075, 54, 68); // 計算重新開始按鈕高度。
  const buttonRect = { x: width / 2 - buttonWidth / 2, y: cardY + cardHeight * 0.76, w: buttonWidth, h: buttonHeight }; // 建立重新開始按鈕的碰撞矩形。
  if (isInside(pointX, pointY, buttonRect)) { // 判斷輸入座標是否位於重新開始按鈕內。
    currentQuestion = 0; // 將題目索引重設為第一題。
    score = 0; // 將答對題數重設為零。
    selectedIndex = -1; // 清除選項選取狀態。
    hasAnswered = false; // 清除目前題目已作答狀態。
    quizFinished = false; // 回到測驗進行畫面。
  } // 結束重新開始按鈕碰撞判斷。
} // 結束重新開始按鈕輸入處理函式。

// p5.js 在滑鼠按下時會自動呼叫此函式。
function mousePressed() { // 定義滑鼠輸入事件函式。
  handlePointer(mouseX, mouseY); // 將滑鼠座標交給共用輸入處理函式。
  return false; // 阻止瀏覽器進行額外的預設滑鼠行為。
} // 結束滑鼠輸入事件函式。

// p5.js 在觸控開始時會自動呼叫此函式。
function touchStarted() { // 定義觸控輸入事件函式。
  handlePointer(mouseX, mouseY); // 將觸控座標交給共用輸入處理函式。
  return false; // 阻止行動裝置滑動畫面造成頁面捲動。
} // 結束觸控輸入事件函式。

// p5.js 在瀏覽器視窗尺寸改變時會自動呼叫此函式。
function windowResized() { // 定義視窗尺寸變更事件函式。
  resizeCanvas(windowWidth, windowHeight); // 重新調整畫布以符合新的視窗大小。
} // 結束視窗尺寸變更事件函式。


```
:::


---

## 學習2：網頁設定為響應式網頁

https://cfchen58.synology.me/115/week4/stage2/

**這個階段的目標：** 讓網站在電腦、平板、手機（直向與橫向）都能正常顯示，視窗大小改變時版面自動調整。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）
![螢幕擷取畫面 2026-10-08 144156](https://hackmd.io/_uploads/Skc8ohNjze.png)

![學習2截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習2的程式碼
```javascript=
// 宣告目前所在的測驗題目索引。
let currentQuestion = 0; // 將第一題設定為目前題目。
let score = 0; // 儲存使用者目前答對的題數。
let selectedIndex = -1; // 記錄目前選取的選項索引。
let hasAnswered = false; // 記錄目前題目是否已經作答。
let quizFinished = false; // 記錄五題是否已經全部完成。
let lastInputTime = 0; // 記錄最近一次輸入時間以避免觸控重複觸發。

// 建立五題 p5.js 基礎指令選擇題資料。
const questions = [ // 宣告所有測驗題目的陣列。
  { // 建立第一題的物件資料。
    prompt: "哪一個函式可以在 p5.js 中建立畫布？", // 設定第一題題目文字。
    options: ["createCanvas()", "makeCanvas()", "newCanvas()", "canvas()"], // 設定第一題四個選項。
    answer: 0 // 設定第一題的正確選項索引。
  }, // 結束第一題物件。
  { // 建立第二題的物件資料。
    prompt: "哪一個函式會在 p5.js 程式開始時執行一次？", // 設定第二題題目文字。
    options: ["start()", "setup()", "begin()", "init()"], // 設定第二題四個選項。
    answer: 1 // 設定第二題的正確選項索引。
  }, // 結束第二題物件。
  { // 建立第三題的物件資料。
    prompt: "哪一個函式通常會在 p5.js 中持續重複執行？", // 設定第三題題目文字。
    options: ["loopOnce()", "repeat()", "draw()", "run()"], // 設定第三題四個選項。
    answer: 2 // 設定第三題的正確選項索引。
  }, // 結束第三題物件。
  { // 建立第四題的物件資料。
    prompt: "哪一個函式可以在畫布上畫出圓形？", // 設定第四題題目文字。
    options: ["circle()", "ellipse()", "round()", "oval()"], // 設定第四題四個選項。
    answer: 1 // 設定第四題的正確選項索引。
  }, // 結束第四題物件。
  { // 建立第五題的物件資料。
    prompt: "哪一個函式可以設定圖形的填滿顏色？", // 設定第五題題目文字。
    options: ["paint()", "background()", "fill()", "colorFill()"], // 設定第五題四個選項。
    answer: 2 // 設定第五題的正確選項索引。
  } // 結束第五題物件。
]; // 結束測驗題目陣列。

// p5.js 在程式開始時會自動呼叫此函式。
function setup() { // 定義初始化函式。
  createCanvas(windowWidth, windowHeight); // 建立符合視窗大小的全螢幕畫布。
  pixelDensity(Math.min(window.devicePixelRatio || 1, 2)); // 限制像素密度以兼顧清晰度與效能。
  textFont("sans-serif"); // 設定不依賴外部素材的系統字型。
  textAlign(CENTER, CENTER); // 將文字預設設定為水平與垂直置中。
  rectMode(CORNER); // 將矩形繪製模式設定為左上角定位。
  noStroke(); // 預設關閉圖形外框線。
  describe("p5.js 基礎指令五題選擇題測驗介面"); // 提供畫布的無障礙文字描述。
} // 結束初始化函式。

// p5.js 會持續呼叫此函式來繪製畫面。
function draw() { // 定義每一幀的繪圖函式。
  drawBackground(); // 繪製適應全螢幕的背景。
  if (quizFinished) { // 判斷是否已經完成全部題目。
    drawResultScreen(); // 完成測驗時繪製成績畫面。
  } else { // 處理尚未完成測驗的情況。
    drawQuizScreen(); // 尚未完成時繪製目前題目的畫面。
  } // 結束完成狀態判斷。
} // 結束每一幀的繪圖函式。

// 繪製不依賴外部圖片的柔和背景。
function drawBackground() { // 定義背景繪製函式。
  background("#f7f9fc"); // 使用淺色背景清除上一幀畫面。
  noStroke(); // 確保背景裝飾不顯示外框線。
  fill("#e8f1f5"); // 設定左上方裝飾圓的顏色。
  circle(width * 0.08, height * 0.08, Math.min(width, height) * 0.22); // 繪製左上方裝飾圓。
  fill("#eef3ff"); // 設定右下方裝飾圓的顏色。
  circle(width * 0.93, height * 0.91, Math.min(width, height) * 0.3); // 繪製右下方裝飾圓。
} // 結束背景繪製函式。

// 取得依照視窗尺寸計算的版面資訊。
function getLayout() { // 定義響應式版面計算函式。
  const narrow = width < 640; // 判斷目前是否為窄螢幕版面。
  const margin = constrain(width * 0.07, 18, 72); // 計算左右內容邊界距離。
  const contentWidth = width - margin * 2; // 計算主要內容可使用寬度。
  const headerY = constrain(height * 0.09, 52, 88); // 計算標題垂直位置。
  const cardY = narrow ? height * 0.17 : height * 0.19; // 計算題目卡片的垂直位置。
  const cardHeight = narrow ? height * 0.22 : height * 0.18; // 計算題目卡片高度。
  const optionGap = narrow ? 12 : 18; // 計算選項之間的間距。
  const columns = narrow ? 1 : 2; // 窄螢幕使用單欄，寬螢幕使用雙欄。
  const optionWidth = columns === 1 ? contentWidth : (contentWidth - optionGap) / 2; // 計算每個選項寬度。
  const optionHeight = narrow ? constrain(height * 0.075, 54, 70) : constrain(height * 0.105, 64, 86); // 計算每個選項高度。
  const optionStartY = cardY + cardHeight + (narrow ? 20 : 28); // 計算第一個選項的垂直位置。
  const buttonWidth = constrain(contentWidth * 0.42, 150, 260); // 計算下一題按鈕寬度。
  const buttonHeight = constrain(height * 0.075, 54, 68); // 計算下一題按鈕高度。
  const buttonY = height - margin - buttonHeight; // 計算下一題按鈕垂直位置。
  return { margin, contentWidth, headerY, cardY, cardHeight, optionGap, columns, optionWidth, optionHeight, optionStartY, buttonWidth, buttonHeight, buttonY }; // 回傳完整響應式版面資訊。
} // 結束版面計算函式。

// 取得指定選項在目前視窗中的矩形範圍。
function getOptionRect(index) { // 定義選項位置計算函式。
  const layout = getLayout(); // 取得目前視窗的版面資訊。
  const column = index % layout.columns; // 計算選項所在的欄位。
  const row = Math.floor(index / layout.columns); // 計算選項所在的列數。
  const x = layout.margin + column * (layout.optionWidth + layout.optionGap); // 計算選項左側座標。
  const y = layout.optionStartY + row * (layout.optionHeight + layout.optionGap); // 計算選項上方座標。
  return { x, y, w: layout.optionWidth, h: layout.optionHeight }; // 回傳選項矩形資料。
} // 結束選項位置計算函式。

// 繪製目前的測驗題目畫面。
function drawQuizScreen() { // 定義測驗畫面繪製函式。
  const layout = getLayout(); // 取得目前視窗的版面資訊。
  const question = questions[currentQuestion]; // 取得目前題目的資料。
  const baseSize = constrain(Math.min(width, height) * 0.04, 16, 28); // 計算適合視窗的基準字體大小。
  fill("#183642"); // 設定主標題文字顏色。
  textStyle(BOLD); // 將主標題設定為粗體。
  textSize(constrain(baseSize * 1.3, 22, 36)); // 設定主標題文字大小。
  text("p5.js 指令小測驗", width / 2, layout.headerY); // 顯示測驗主標題。
  textStyle(NORMAL); // 將後續文字恢復為一般字體。
  fill("#52727b"); // 設定進度文字顏色。
  textSize(constrain(baseSize * 0.65, 13, 18)); // 設定進度文字大小。
  text(`第 ${currentQuestion + 1} 題／共 ${questions.length} 題`, width / 2, layout.headerY + baseSize * 1.4); // 顯示目前答題進度。
  drawQuestionCard(question, layout, baseSize); // 繪製目前題目的文字卡片。
  for (let index = 0; index < question.options.length; index += 1) { // 逐一繪製四個選項。
    drawOption(question, index, baseSize); // 繪製指定索引的選項。
  } // 結束四個選項的繪製迴圈。
  if (hasAnswered) { // 判斷使用者是否已經選擇答案。
    drawFeedback(question, layout, baseSize); // 顯示作答結果與提示文字。
    drawNextButton(layout, baseSize); // 顯示作答後才能使用的下一題按鈕。
  } // 結束作答狀態判斷。
} // 結束測驗畫面繪製函式。

// 繪製題目文字卡片。
function drawQuestionCard(question, layout, baseSize) { // 定義題目卡片繪製函式。
  fill("#ffffff"); // 設定題目卡片填滿色。
  rect(layout.margin, layout.cardY, layout.contentWidth, layout.cardHeight, 20); // 繪製圓角題目卡片。
  fill("#183642"); // 設定題目文字顏色。
  textStyle(BOLD); // 將題目文字設定為粗體。
  textSize(constrain(baseSize * 0.92, 18, 27)); // 設定題目文字大小。
  drawWrappedText(question.prompt, width / 2, layout.cardY + layout.cardHeight / 2, layout.contentWidth - 36, baseSize * 1.3); // 以自動換行方式繪製題目。
  textStyle(NORMAL); // 將文字樣式恢復為一般。
} // 結束題目卡片繪製函式。

// 繪製單一選項並套用作答動畫與顏色。
function drawOption(question, index, baseSize) { // 定義選項繪製函式。
  const rectData = getOptionRect(index); // 取得指定選項的原始矩形位置。
  let offsetX = 0; // 初始化選項的水平動畫位移。
  let offsetY = 0; // 初始化選項的垂直動畫位移。
  const isCorrect = index === question.answer; // 判斷目前選項是否為正確答案。
  const isSelected = index === selectedIndex; // 判斷目前選項是否為使用者選取答案。
  if (hasAnswered && !isCorrect && isSelected) { // 判斷是否為答錯且被選取的選項。
    offsetX = Math.sin(frameCount * 0.35) * 9; // 讓答錯選項持續左右移動。
  } // 結束答錯選項動畫判斷。
  if (hasAnswered && !isCorrect && selectedIndex !== question.answer && isCorrect) { // 保留正確答案動畫判斷的結構。
    offsetY = 0; // 讓不符合條件的分支維持零位移。
  } // 結束不會觸發的相容判斷。
  if (hasAnswered && selectedIndex !== question.answer && isCorrect) { // 判斷答錯後的正確答案選項。
    offsetY = Math.sin(frameCount * 0.32) * 9; // 讓正確答案選項持續上下跳動。
  } // 結束正確答案動畫判斷。
  push(); // 保存目前的繪圖樣式與座標狀態。
  translate(offsetX, offsetY); // 套用選項的動畫位移。
  if (!hasAnswered) { // 判斷尚未作答的狀態。
    fill("#ffffff"); // 將未作答選項設定為白色背景。
  } else if (isCorrect && selectedIndex !== question.answer) { // 判斷答錯後的正確選項。
    fill("#e0fbfc"); // 將正確答案背景設定為指定的淺藍色。
  } else if (isSelected && !isCorrect) { // 判斷使用者答錯的選項。
    fill("#ff758f"); // 將答錯選項背景設定為指定的粉紅色。
  } else if (isSelected && isCorrect) { // 判斷使用者答對並選中正確選項的狀態。
    fill("#b7efc5"); // 將答對的選項設定為明確的綠色背景。
  } else { // 處理作答後未被選取的其他選項。
    fill("#ffffff"); // 將其他選項維持為白色背景。
  } // 結束選項背景顏色判斷。
  rect(rectData.x, rectData.y, rectData.w, rectData.h, 16); // 繪製圓角選項按鈕。
  if (isSelected) { // 判斷目前選項是否被使用者選取。
    stroke("#183642"); // 為選取的選項加上深色外框。
    strokeWeight(3); // 設定選取外框的線條寬度。
    noFill(); // 設定外框繪製時不填滿內部。
    rect(rectData.x, rectData.y, rectData.w, rectData.h, 16); // 繪製選取選項的外框。
    noStroke(); // 恢復關閉外框線以免影響下一個圖形。
  } // 結束選取外框判斷。
  fill("#183642"); // 設定選項文字顏色。
  textSize(constrain(baseSize * 0.72, 15, 22)); // 設定選項文字大小。
  drawWrappedText(`${String.fromCharCode(65 + index)}. ${question.options[index]}`, rectData.x + rectData.w / 2, rectData.y + rectData.h / 2, rectData.w - 28, baseSize); // 繪製選項編號與文字。
  pop(); // 還原繪圖樣式與座標狀態。
} // 結束單一選項繪製函式。

// 繪製作答後的明確回饋文字。
function drawFeedback(question, layout, baseSize) { // 定義作答回饋繪製函式。
  const correct = selectedIndex === question.answer; // 判斷本題是否答對。
  const feedbackY = layout.buttonY - baseSize * 0.9; // 計算回饋文字的垂直位置。
  fill(correct ? "#237a57" : "#b4233d"); // 依照答題結果設定回饋文字顏色。
  textStyle(BOLD); // 將回饋文字設定為粗體。
  textSize(constrain(baseSize * 0.75, 15, 21)); // 設定回饋文字大小。
  if (correct) { // 判斷答對時的回饋內容。
    text("答對了！你選擇的答案正確。", width / 2, feedbackY); // 明確顯示答對結果。
  } else { // 處理答錯時的回饋內容。
    text(`答錯了！正確答案是 ${String.fromCharCode(65 + question.answer)}。`, width / 2, feedbackY); // 明確顯示答錯與正確答案。
  } // 結束答題回饋判斷。
  textStyle(NORMAL); // 將文字樣式恢復為一般。
} // 結束作答回饋繪製函式。

// 繪製作答後出現的下一題按鈕。
function drawNextButton(layout, baseSize) { // 定義下一題按鈕繪製函式。
  const buttonX = width / 2 - layout.buttonWidth / 2; // 計算下一題按鈕左側座標。
  fill("#247ba0"); // 設定下一題按鈕背景顏色。
  rect(buttonX, layout.buttonY, layout.buttonWidth, layout.buttonHeight, 16); // 繪製圓角下一題按鈕。
  fill("#ffffff"); // 設定下一題按鈕文字顏色。
  textStyle(BOLD); // 將按鈕文字設定為粗體。
  textSize(constrain(baseSize * 0.78, 16, 22)); // 設定按鈕文字大小。
  text(currentQuestion === questions.length - 1 ? "查看結果" : "下一題", width / 2, layout.buttonY + layout.buttonHeight / 2); // 顯示下一題或查看結果文字。
  textStyle(NORMAL); // 將文字樣式恢復為一般。
} // 結束下一題按鈕繪製函式。

// 繪製五題完成後的成績畫面。
function drawResultScreen() { // 定義結果畫面繪製函式。
  const baseSize = constrain(Math.min(width, height) * 0.045, 18, 32); // 計算結果畫面的基準字體大小。
  const cardWidth = Math.min(width * 0.86, 620); // 計算結果卡片寬度。
  const cardHeight = Math.min(height * 0.58, 430); // 計算結果卡片高度。
  const cardX = width / 2 - cardWidth / 2; // 計算結果卡片左側座標。
  const cardY = height / 2 - cardHeight / 2; // 計算結果卡片上方座標。
  fill("#ffffff"); // 設定結果卡片背景顏色。
  rect(cardX, cardY, cardWidth, cardHeight, 24); // 繪製圓角結果卡片。
  fill("#183642"); // 設定結果標題文字顏色。
  textStyle(BOLD); // 將結果標題設定為粗體。
  textSize(constrain(baseSize * 1.35, 26, 42)); // 設定結果標題文字大小。
  text("測驗完成！", width / 2, cardY + cardHeight * 0.24); // 顯示測驗完成標題。
  fill("#247ba0"); // 設定分數文字顏色。
  textSize(constrain(baseSize * 1.8, 40, 70)); // 設定分數文字大小。
  text(`${score}／${questions.length} 題答對`, width / 2, cardY + cardHeight * 0.48); // 顯示答對題數。
  fill("#52727b"); // 設定鼓勵文字顏色。
  textSize(constrain(baseSize * 0.7, 15, 21)); // 設定鼓勵文字大小。
  text(score === questions.length ? "太棒了！全部答對！" : "再練習一次，你會越來越熟悉！", width / 2, cardY + cardHeight * 0.64); // 顯示依分數變化的鼓勵文字。
  const buttonWidth = constrain(cardWidth * 0.5, 170, 260); // 計算重新開始按鈕寬度。
  const buttonHeight = constrain(height * 0.075, 54, 68); // 計算重新開始按鈕高度。
  const buttonX = width / 2 - buttonWidth / 2; // 計算重新開始按鈕左側座標。
  const buttonY = cardY + cardHeight * 0.76; // 計算重新開始按鈕上方座標。
  fill("#247ba0"); // 設定重新開始按鈕背景顏色。
  rect(buttonX, buttonY, buttonWidth, buttonHeight, 16); // 繪製圓角重新開始按鈕。
  fill("#ffffff"); // 設定重新開始按鈕文字顏色。
  textSize(constrain(baseSize * 0.78, 16, 22)); // 設定重新開始按鈕文字大小。
  text("重新開始", width / 2, buttonY + buttonHeight / 2); // 顯示重新開始按鈕文字。
  textStyle(NORMAL); // 將文字樣式恢復為一般。
} // 結束結果畫面繪製函式。

// 將沒有空格的中文題目或選項依照寬度切成多行。
function wrapText(message, maxWidth) { // 定義文字換行函式。
  const lines = []; // 建立儲存每一行文字的陣列。
  let currentLine = ""; // 建立目前正在組合的文字行。
  for (const character of message) { // 逐字檢查文字是否超過可用寬度。
    const testLine = currentLine + character; // 暫時組合加入下一個字後的文字行。
    if (textWidth(testLine) > maxWidth && currentLine.length > 0) { // 判斷加入新字後是否超過寬度。
      lines.push(currentLine); // 將已經完成的文字行存入陣列。
      currentLine = character; // 以目前字元開始下一行。
    } else { // 處理尚未超過寬度的情況。
      currentLine = testLine; // 將暫時文字行設定為目前文字行。
    } // 結束文字寬度判斷。
  } // 結束逐字檢查迴圈。
  if (currentLine.length > 0) { // 判斷是否還有最後一行尚未存入。
    lines.push(currentLine); // 將最後一行文字存入陣列。
  } // 結束最後一行判斷。
  return lines; // 回傳換行後的文字陣列。
} // 結束文字換行函式。

// 依照行高繪製已經換行的文字。
function drawWrappedText(message, centerX, centerY, maxWidth, lineHeight) { // 定義多行文字繪製函式。
  const lines = wrapText(message, maxWidth); // 取得依寬度切分後的文字行。
  const totalHeight = (lines.length - 1) * lineHeight; // 計算全部文字行的總高度。
  for (let index = 0; index < lines.length; index += 1) { // 逐行繪製換行文字。
    const lineY = centerY - totalHeight / 2 + index * lineHeight; // 計算目前文字行的垂直位置。
    text(lines[index], centerX, lineY); // 繪製目前文字行。
  } // 結束逐行繪製迴圈。
} // 結束多行文字繪製函式。

// 判斷座標是否位於指定矩形內。
function isInside(pointX, pointY, rectData) { // 定義矩形碰撞判斷函式。
  return pointX >= rectData.x && pointX <= rectData.x + rectData.w && pointY >= rectData.y && pointY <= rectData.y + rectData.h; // 回傳座標是否位於矩形內。
} // 結束矩形碰撞判斷函式。

// 處理滑鼠與觸控共用的選取座標。
function handlePointer(pointX, pointY) { // 定義滑鼠與觸控共用的輸入處理函式。
  if (millis() - lastInputTime < 250) { // 判斷是否在短時間內重複觸發輸入。
    return; // 忽略過短時間內的重複輸入。
  } // 結束重複輸入判斷。
  lastInputTime = millis(); // 更新最近一次輸入時間。
  if (quizFinished) { // 判斷目前是否位於結果畫面。
    handleRestartPointer(pointX, pointY); // 將輸入交由重新開始按鈕處理。
    return; // 結束結果畫面的輸入處理。
  } // 結束結果畫面判斷。
  if (!hasAnswered) { // 判斷目前題目是否尚未作答。
    handleOptionPointer(pointX, pointY); // 將輸入交由四個選項處理。
  } else { // 處理已經完成作答的狀態。
    handleNextPointer(pointX, pointY); // 將輸入交由下一題按鈕處理。
  } // 結束作答狀態判斷。
} // 結束共用輸入處理函式。

// 處理尚未作答時的選項點擊或觸控。
function handleOptionPointer(pointX, pointY) { // 定義選項輸入處理函式。
  const question = questions[currentQuestion]; // 取得目前題目的資料。
  for (let index = 0; index < question.options.length; index += 1) { // 逐一檢查四個選項的碰撞範圍。
    const rectData = getOptionRect(index); // 取得目前選項的矩形範圍。
    if (isInside(pointX, pointY, rectData)) { // 判斷輸入座標是否落在目前選項內。
      selectedIndex = index; // 記錄使用者所選的選項索引。
      hasAnswered = true; // 鎖定本題答案並顯示回饋。
      if (selectedIndex === question.answer) { // 判斷使用者是否答對本題。
        score += 1; // 將答對題數增加一題。
      } // 結束答對分數判斷。
      return; // 選到選項後結束檢查迴圈。
    } // 結束選項碰撞判斷。
  } // 結束檢查四個選項的迴圈。
} // 結束選項輸入處理函式。

// 處理下一題或查看結果按鈕的點擊與觸控。
function handleNextPointer(pointX, pointY) { // 定義下一題按鈕輸入處理函式。
  const layout = getLayout(); // 取得目前視窗的版面資訊。
  const buttonRect = { x: width / 2 - layout.buttonWidth / 2, y: layout.buttonY, w: layout.buttonWidth, h: layout.buttonHeight }; // 建立下一題按鈕的碰撞矩形。
  if (isInside(pointX, pointY, buttonRect)) { // 判斷輸入座標是否位於下一題按鈕內。
    if (currentQuestion < questions.length - 1) { // 判斷是否還有下一題可以繼續。
      currentQuestion += 1; // 將題目索引移至下一題。
      selectedIndex = -1; // 清除下一題的選取狀態。
      hasAnswered = false; // 讓下一題回到尚未作答狀態。
    } else { // 處理已經答完第五題的情況。
      quizFinished = true; // 切換至測驗結果畫面。
    } // 結束題目進度判斷。
  } // 結束下一題按鈕碰撞判斷。
} // 結束下一題按鈕輸入處理函式。

// 處理結果畫面重新開始按鈕的點擊與觸控。
function handleRestartPointer(pointX, pointY) { // 定義重新開始按鈕輸入處理函式。
  const cardWidth = Math.min(width * 0.86, 620); // 計算結果卡片寬度。
  const cardHeight = Math.min(height * 0.58, 430); // 計算結果卡片高度。
  const cardY = height / 2 - cardHeight / 2; // 計算結果卡片上方座標。
  const buttonWidth = constrain(cardWidth * 0.5, 170, 260); // 計算重新開始按鈕寬度。
  const buttonHeight = constrain(height * 0.075, 54, 68); // 計算重新開始按鈕高度。
  const buttonRect = { x: width / 2 - buttonWidth / 2, y: cardY + cardHeight * 0.76, w: buttonWidth, h: buttonHeight }; // 建立重新開始按鈕的碰撞矩形。
  if (isInside(pointX, pointY, buttonRect)) { // 判斷輸入座標是否位於重新開始按鈕內。
    currentQuestion = 0; // 將題目索引重設為第一題。
    score = 0; // 將答對題數重設為零。
    selectedIndex = -1; // 清除選項選取狀態。
    hasAnswered = false; // 清除目前題目已作答狀態。
    quizFinished = false; // 回到測驗進行畫面。
  } // 結束重新開始按鈕碰撞判斷。
} // 結束重新開始按鈕輸入處理函式。

// p5.js 在滑鼠按下時會自動呼叫此函式。
function mousePressed() { // 定義滑鼠輸入事件函式。
  handlePointer(mouseX, mouseY); // 將滑鼠座標交給共用輸入處理函式。
  return false; // 阻止瀏覽器進行額外的預設滑鼠行為。
} // 結束滑鼠輸入事件函式。

// p5.js 在觸控開始時會自動呼叫此函式。
function touchStarted() { // 定義觸控輸入事件函式。
  handlePointer(mouseX, mouseY); // 將觸控座標交給共用輸入處理函式。
  return false; // 阻止行動裝置滑動畫面造成頁面捲動。
} // 結束觸控輸入事件函式。

// p5.js 在瀏覽器視窗尺寸改變時會自動呼叫此函式。
function windowResized() { // 定義視窗尺寸變更事件函式。
  resizeCanvas(windowWidth, windowHeight); // 重新調整畫布以符合新的視窗大小。
} // 結束視窗尺寸變更事件函式。


```
:::


---

## 學習3：設定嵌入 Google 字型，網頁文字採用這些字型

https://cfchen58.synology.me/115/week4/stage3/

**這個階段的目標：** 從 Google Fonts 嵌入繁體中文字型，並讓畫布上的題目與選項文字使用這些字型。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習3截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習3的程式碼
```javascript=
//學習3程式碼所在

```
:::


---

## 學習4：設定題庫並抽題顯示題目網頁（CSV 檔案）

https://cfchen58.synology.me/115/week4/stage4/

**這個階段的目標：** 把題目移到 questions.csv，網站讀取題庫後每次隨機抽出 5 題。
**這個階段會修改的檔案：** index.html、sketch.js、questions.csv

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習4截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習4的程式碼
```javascript=
//學習4程式碼所在

```
:::


---

## 學習5：利用 Google Sheets 當題庫

https://cfchen58.synology.me/115/week4/stage5/

**這個階段的目標：** 把題庫放在 Google 試算表，網站直接讀取，老師改試算表，網站題目就跟著更新。
**這個階段會修改的檔案：** index.html、sketch.js（questions.csv 當備用題庫）

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習5截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習5的程式碼
```javascript=
//學習5程式碼所在

```
:::


---

## 我的心得

這五個學習中，哪一個最困難？你是怎麼解決的？（請寫出實際發生的事）

＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿
