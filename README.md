# Sol-Lucky-Draw
Sol Lucky Draw
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>QA 自動回覆</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      background-color: #1a1616;
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    .qa-container {
      background-color: #2c2c2c;
      border: 6px solid #a67c52;
      border-radius: 20px;
      padding: 30px 25px;
      width: 90%;
      max-width: 1000px;
      box-sizing: border-box;
      overflow-y: auto;
      max-height: 95vh;
    }

    .qa-item {
      margin-bottom: 20px;
    }

    .question {
      cursor: pointer;
      font-size: 1.5rem;
      color: #FFC107;
      margin-bottom: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background-color: #444;
      padding: 12px 16px;
      border-radius: 10px;
      transition: background 0.3s;
    }

    .question:hover {
      background-color: #555;
    }

    .arrow {
      font-size: 1.2rem;
      color: #FFC107;
    }

    .answer {
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.4s ease, padding 0.3s ease;
      font-size: 1.1rem;
      color: #ffffff;
      padding-left: 20px;
      line-height: 1.6;
      background-color: #3a3a3a;
      border-radius: 8px;
      padding: 0 16px;
    }

    .answer.open {
      max-height: 800px;
      padding: 15px 16px;
    }

    @media (max-width: 600px) {
      .qa-container {
        padding: 20px 15px;
      }
      .question {
        font-size: 1.2rem;
      }
      .answer {
        font-size: 1rem;
      }
    }
  </style>
</head>
<body>

  <div class="qa-container" id="qa">
    <!-- QA 內容將由 JS 注入 -->
  </div>

  <script>
    const qaData = [
      {
        q: "是否有停車位・充電設施？",
        a: `飯店提供住客免費汽車平面停車場於地下一樓（限高2.2米），您可於入住當日直接進入地下一樓停放，再搭乘電梯至一樓櫃檯登記入住，因車位有限，無法預做保留。<br><br>
            若館內停車場停滿，您可自行停放至距離飯店步行5分鐘的二個公有停車場，東大路橋下(入口在中央路)或府後停車場，飯店將會支付您的停車費(請於退房11：00前於櫃檯索取停車時數抵用後再行取車)。<br><br>
            ⭐ 新春及週末尖峰時段，停車需求量大，如遇等待車位狀況，造成您的不便，敬請見諒。<br>
            ⭐ 團體及特殊優惠專案，"不提供" 停車時數抵用。<br>
            ⭐ 車位僅供館內住宿客人停放，非住客停放須收取$500元停車費用。<br>
            ⭐ 本館停車場無附設充電設施。`
      },
      {
        q: "早餐內容・供應時間？",
        a: `▪ 歐式Buffet，提供中式、西式與日式餐點。<br>
            ▪ 用餐時間 6:30 ~ 10:00，核對房號及用餐人數即可用餐。<br>
            ⭐ 遇年節尖峰用餐時間限時 1 小時。`
      },
      {
        q: "是否可提供嬰兒床・床圍？",
        a: `飯店備有嬰兒床、床圍免費租用，如需預訂使用請注意以下事項：<br>
            ▪ 因安全考慮，嬰兒床僅限1歲以下幼兒使用。<br>
            ▪ 一歲以上幼童建議使用床圍，並須入住前安裝。<br>
            ▪ 數量有限，請於訂房同時提前告知。<br>
            ⭐ 精緻客房（標準雙人房）無法提供嬰兒床服務。`
      },
      {
        q: "是否可提供嬰兒澡盆・奶瓶消毒鍋？",
        a: `▪ 本飯店備有嬰兒澡盆(附小板凳 )、奶瓶消毒鍋免費租用。<br>
            ▪ 因數量不多，如有需要請事先預訂保留。`
      },
       {
        q: "浴室內是否有浴缸・是否有乾溼分離？",
        a: `▪ 精緻客房（標準雙人房）僅有座式小浴缸。<br>
            ▪ 豪華客房以上房型皆附 (單人) 標準型浴缸。<br>
            ▪ 除無障礙房以外，全館客房浴室皆為乾溼分離。`
      },
      {
        q: "兒童入住加價方式？",
        a: `▪ 1歲以下嬰兒可使用嬰兒床（數量有限，需預訂）<br>
            ▪ 109公分以下免收費<br>
            ▪ 110～139公分加收220元<br>
            ▪ 140公分以上比照成人，建議加床<br>
            ▪ 加床含早餐880元（新春1200元），不含早餐550元（新春1000元）`
      },
      {
        q: "成人入住加價方式？",
        a: `▪ 含早餐加床：每人880元（新春1200元）<br>
            僅增加住宿人數：500元（新春800元）<br><br>
            ▪ 不含早餐加床：550元（新春1000元）<br>
            僅增加住宿人數：400元（新春500元）<br><br>
         ⭐ 精緻客房（標準雙人房）無法提供加床服務。`
      },
      {
        q: "平日價・假日價定義說明？",
        a: `▪ 平日：星期日~星期五<br>▪ 假日：星期六、及特殊節日`
      },

 {
        q: "是否有住宿年齡限制？",
        a: `▪ 須年滿 20 歲才能辦理入住，未滿 20 歲入住需填寫家長同意書。`
      },

       {
        q: "是否可使用國旅卡？",
        a: `▪ 很抱歉，本飯店非國民旅遊卡特約商店。`
      },
      
      {
        q: "詢問大眾交通・步行・接駁服務？",
        a: `▪ 台鐵新竹站步行約8分鐘抵達飯店（東門國小後門）<br>
            ▪ 高鐵新竹站 → 步行2分鐘過空橋轉乘台鐵(六家站)至新竹站 → 步行8分鐘抵達飯店<br>
            ▪ 本飯店無免費接駁服務`
      },
      {
        q: "關於入住時間・行李寄存？",
        a: `▪ 入住時間為15:00後，退房時間為翌日11:00前。<br>
         ⭐ 不指定房型優惠專案"限定19:00 後入住"。<br>
            ▪ 入住前與退房後皆可於櫃檯寄放行李。`
      }
    ];

    const container = document.getElementById('qa');

    qaData.forEach((item, index) => {
      const qaItem = document.createElement('div');
      qaItem.className = 'qa-item';

      const question = document.createElement('div');
      question.className = 'question';
      question.innerHTML = `・${item.q} <span class="arrow">▼</span>`;
      question.onclick = () => toggleAnswer(index);

      const answer = document.createElement('div');
      answer.className = 'answer';
      answer.innerHTML = item.a;

      qaItem.appendChild(question);
      qaItem.appendChild(answer);
      container.appendChild(qaItem);
    });

    function toggleAnswer(index) {
      const answers = document.querySelectorAll('.answer');
      const arrows = document.querySelectorAll('.arrow');
      const answer = answers[index];
      const arrow = arrows[index];
      const isOpen = answer.classList.contains('open');

      answer.classList.toggle('open');
      arrow.textContent = isOpen ? '▼' : '▲';
    }

    // 預設展開第一個問題
    window.onload = () => toggleAnswer(0);
  </script>
</body>
</html>
