---
title: Oracles
description: اوراکل‌ها قراردادهای هوشمند اتریوم را با دسترسی به داده‌های دنیای واقعی، باز کردن موارد استفاده بیشتر و ارزش بیشتر برای کاربران فراهم می‌کند.
lang: fa
---

اوراکل‌ها برنامه‌هایی هستند که فیدهای داده تولید می‌کنند که منابع داده خارج از زنجیره را برای قراردادهای هوشمند در دسترس بلاک‌چین قرار می‌دهند. این امر ضروری است زیرا قراردادهای هوشمند مبتنی بر اتریوم به طور پیش فرض نمی‌توانند به اطلاعات ذخیره شده خارج از شبکه بلاک‌چین دسترسی داشته باشند.

دادن قابلیت اجرای قراردادهای هوشمند با استفاده از داده‌های خارج از زنجیره، کاربرد و ارزش برنامه‌های غیرمتمرکز را گسترش می‌دهد. به عنوان مثال، بازارهای پیش‌بینی زنجیره‌ای به اوراکل‌ها برای ارائه اطلاعاتی درباره نتایجی که برای اعتبارسنجی پیش‌بینی‌های کاربر استفاده می‌کنند، متکی هستند. فرض کنید آلیس 20 ETH بر روی این که چه کسی رئیس جمهور آینده ایالات متحده آمریکا خواهد شد، شرط ببندد.  در آن صورت، پیش‌بینی بازار به یک اوراکل برای تأیید نتایج انتخابات و تعیین اینکه آیا آلیس (Alice) واجد شرایط پرداخت است یا خیر، نیاز دارد.

## موارد مورد نیاز {#prerequisites}

این صفحه فرض می‌کند که خواننده با مفاهیم پایه اتریوم از جمله [گره‌ها](/developers/docs/nodes-and-clients/)، [مکانیزم اجماع](/developers/docs/consensus-mechanisms/) و [ماشین مجازی اتریوم یا EVM](/developers/docs/evm/) آشنا می‎‌باشد. همچنین شما باید درک خوبی از [قراردادهای هوشمند](/developers/docs/smart-contracts/) و [ساختار قراردادهای هوشمند](/developers/docs/smart-contracts/anatomy/)، مخصوصاً [رویدادها](/glossary/#events) داشته باشید.

## اوراکل بلاک‌چین چیست؟ {#what-is-a-blockchain-oracle}

اوراکل‌ها برنامه‌هایی هستند که اطلاعات خارجی (یعنی اطلاعات ذخیره شده خارج از زنجیره) را به قراردادهای هوشمند در حال اجرا بر روی بلاک‌چین منبع، تأیید و انتقال می‌دهند. علاوه بر «پول کردن» داده‌های خارج از زنجیره و پخش آن در اتریوم، اوراکل‌ها همچنین می‌توانند اطلاعات را از بلاک‌چین به سیستم‌های خارجی «پوش» کنند، به‌عنوان مثال، زمانی که کاربر هزینه‌ای را از طریق تراکنش اتریوم ارسال می‌کند، قفل هوشمند را باز کند.

بدون اوراکل، یک قرارداد هوشمند به طور کامل به داده‌های زنجیره‌ای محدود می‌شود.

اوراکل‌ها بر اساس منبع داده‌ها (یک یا چند منبع)، مدل‌های قابل اعتماد (متمرکز یا غیرمتمرکز)، و معماری سیستم (خواندن فوری، انتشار، اشتراک، و درخواست-پاسخ) متفاوت هستند. ما همچنین می‌توانیم بین اوراکل‌ها تمایز قائل شویم که آیا آنها داده‌های خارجی را برای استفاده توسط قراردادهای روی زنجیره (اوراکل‌های ورودی) بازیابی می‌کنند، اطلاعات را از زنجیره بلاک به برنامه‌های خارج از زنجیره (اوراکل‌های خروجی) ارسال می‌کنند یا وظایف محاسباتی را خارج از زنجیره انجام می‌دهند (اوراکل‌های محاسباتی).

## چرا قراردادهای هوشمند به اوراکل نیاز دارند؟ {#why-do-smart-contracts-need-oracles}

بسیاری از توسعه دهندگان قراردادهای هوشمند را به عنوان کدی می بینند که در آدرس‌های خاصی در بلاک‌چین اجرا می‌شود. با این حال، [دیدگاه کلی‌تر قراردادهای هوشمند](/smart-contracts/) این است که آنها برنامه‌های نرم‌افزاری خود-اجرای هستند که قادر به اجرای توافقات بین طرفین پس از برآورده شدن شرایط خاص هستند - از این رو به این اصطلاح قراردادهای هوشمند می‌گوییم.»

اما استفاده از قراردادهای هوشمند برای اجرای توافقات بین افراد، با توجه به اینکه اتریوم قطعی است، ساده نیست. یک [سیستم قطعی](https://en.wikipedia.org/wiki/Deterministic_algorithm) سیستمی است که همیشه نتایج یکسانی را با توجه به وضعیت اولیه و یک ورودی خاص تولید می‌کند، به این معنی که تصادفی یا تغییر در فرآیند محاسبه خروجی‌ها از ورودی ها وجود ندارد.

برای دستیابی به اجرای قطعی، بلاک چین‌ها گره‌ها را به اجماع در مورد سؤالات باینری ساده (درست/نادرست) با استفاده از _فقط_ داده‌های ذخیره شده در خود بلاک چین محدود می‌کنند. نمونه‌هایی از این گونه سوالات عبارتند از:

- "آیا مالک حساب (که با یک کلید عمومی مشخص می‌شود) این تراکنش را با کلید خصوصی جفت شده امضا کرده است؟"
- "آیا این حساب دارای وجوه کافی برای پوشش تراکنش است؟"
- «آیا این معامله در چارچوب این قرارداد هوشمند معتبر است؟» و غیره.

اگر بلاک‌چین‌ها اطلاعاتی را از منابع خارجی (یعنی از دنیای واقعی) دریافت می‌کردند، دستیابی به جبر غیرممکن خواهد بود و از توافق گره‌ها در مورد اعتبار تغییرات در وضعیت بلاک چین جلوگیری می‌کند. به عنوان مثال یک قرارداد هوشمند را در نظر بگیرید که یک تراکنش را بر اساس نرخ ارز فعلی اتر-USD به دست آمده از یک API قیمت سنتی انجام می‌دهد. این رقم احتمالاً اغلب تغییر می‌کند (غیر از اینکه API ممکن است منسوخ یا هک شود)، به این معنی که گره یا همان نودهایی که کد قرارداد یکسانی را اجرا می‌کنند به نتایج متفاوتی می‌رسند.

برای یک بلاک چین عمومی مانند اتریوم، با هزاران گره در سراسر جهان که تراکنش‌ها را پردازش می‌کنند، جبرگرایی بسیار مهم است. بدون هیچ مرجع مرکزی به عنوان منبع حقیقت، گره‌ها به مکانیزم‌هایی برای رسیدن به همان حالت پس از اعمال همان تراکنش‌ها نیاز دارند. موردی که به موجب آن گره یا نود A کد قرارداد هوشمند را اجرا می‌کند و در نتیجه "3" دریافت می‌کند، در حالی که گره یا نود B پس از اجرای همان تراکنش، "7" را دریافت می‌کند، باعث می‌شود اجماع از بین برود و ارزش اتریوم به عنوان یک پلتفرم محاسباتی غیرمتمرکز را حذف کند.

این سناریو همچنین مشکل طراحی بلاک‌چین برای استخراج اطلاعات از منابع خارجی را مطرح می‌کند. با این حال، اوراکل این مشکل را با گرفتن اطلاعات از منابع خارج از زنجیره و ذخیره آن در بلاک‌چین برای مصرف قراردادهای هوشمند حل می‌کند. از آنجایی که اطلاعات ذخیره شده در زنجیره غیرقابل تغییر و در دسترس عموم است، گره‌ یا نودهای اتریوم می‌توانند با خیال راحت از داده‌های خارج از زنجیره وارد شده اوراکل برای محاسبه تغییرات حالت بدون اجماع استفاده کنند.

برای انجام این کار، اوراکل معمولاً از یک قرارداد هوشمند در حال اجرا بر روی زنجیره و برخی از اجزای خارج از زنجیره تشکیل شده است. قرارداد روی زنجیره درخواست‌هایی برای داده‌ها از سایر قراردادهای هوشمند دریافت می‌کند، که آن‌ها را به جزء خارج از زنجیره (به نام گره اوراکل) ارسال می‌کند. این گره یا نود اوراکل می‌تواند منابع داده را جستجو کند - برای مثال با استفاده از رابط‌های برنامه‌نویسی کاربردی (API) - و تراکنش‌هایی را برای ذخیره داده‌های درخواستی در فضای ذخیره‌سازی قرارداد هوشمند ارسال کند.

اساساً یک اوراکل بلاک چین، شکاف اطلاعاتی بین بلاک چین و محیط خارجی را پر کرده و "قراردادهای هوشمند ترکیبی" ایجاد می‌کند. قرارداد هوشمند ترکیبی قراردادی است که بر اساس ترکیبی از کد قرارداد درون زنجیره‌ای و زیرساخت خارج از زنجیره عمل می‌کند. بازارهای پیش‌بینی غیرمتمرکز نمونه‌ای عالی از قراردادهای هوشمند ترکیبی هستند. نمونه‌های دیگر ممکن است شامل قراردادهای هوشمند بیمه محصولات باشد که زمانی پرداخت می‌شوند که مجموعه‌ای از اوراکل‌ها تشخیص دهند که پدیده‌های آب و هوایی خاصی رخ داده است.

## مشکل اوراکل چیست؟ {#the-oracle-problem}

اوراکل‌ها یک مشکل مهم را حل می‌کنند، اما برخی از عوارض را نیز معرفی می‌کنند، به عنوان مثال:

- چگونه بررسی کنیم که اطلاعات وارد شده از منبع صحیح استخراج شده یا دستکاری نشده است؟

- چگونه اطمینان حاصل کنیم که این داده‌ها همیشه در دسترس هستند و به طور منظم به روز می‌شوند؟

به اصطلاح "مشکل اوراکل" مشکلاتی را که با استفاده از اوراکل‌های بلاک چین برای ارسال ورودی به قراردادهای هوشمند به وجود می‌آید را نشان می‌دهد. برای اجرای صحیح قرارداد هوشمند، داده‌های اوراکل باید صحیح باشد. علاوه بر این، نیاز به «اعتماد» به اپراتورهای اوراکل برای ارائه اطلاعات دقیق، جنبه «بدون نیاز به اعتماد» قراردادهای هوشمند را تضعیف می‌کند.

اوراکل‌های مختلف راه‌حل‌های متفاوتی برای مشکل اوراکل ارائه می‌کنند که در ادامه به بررسی آن‌ها می‌پردازیم. اوراکل‌ها معمولاً بر این اساس ارزیابی می‌شوند که چگونه می‌توانند چالش‌های زیر را مدیریت کنند:

1. **صحت**: اوراکل نباید باعث شود که قراردادهای هوشمند تغییرات حالت را بر اساس داده‌های خارج از زنجیره نامعتبر ایجاد کنند. اوراکل باید _اصالت_ و _یکپارچگی_ داده‌ها را تضمین کند. اصالت به این معنی است که داده‌ها از منبع صحیح دریافت شده‌اند، در حالی که یکپارچگی به این معنی است که داده‌ها قبل از ارسال روی زنجیره دست نخورده باقی مانده‌اند (یعنی تغییر نکرده‌اند).

2. **در دسترس بودن**: اوراکل نباید قراردادهای هوشمند را از اجرای اقدامات و ایجاد تغییرات حالت به تاخیر بیاندازد یا از آن جلوگیری کند. این بدان معناست که داده‌های یک اوراکل باید بدون وقفه _در صورت درخواست در دسترس باشد_.

3. **سازگاری انگیزه**: اوراکل باید ارائه دهندگان داده‌های خارج از زنجیره را تشویق کند تا اطلاعات صحیح را به قراردادهای هوشمند ارسال کنند. سازگاری انگیزه شامل _قابلیت انتساب_ و _پاسخگویی_ است. قابلیت انتساب امکان پیوند بخشی از اطلاعات خارجی را به ارائه‌دهنده آن فراهم می‌کند، در حالی که مسئولیت‌پذیری ارائه‌دهندگان داده‌ها را به اطلاعاتی که می‌دهند پیوند می‌دهد، بنابراین می‌توانند بر اساس کیفیت اطلاعات ارائه‌شده پاداش یا جریمه شوند.

## سرویس اوراکل بلاک چین چگونه کار می‌کند؟ {#how-does-a-blockchain-oracle-service-work}

### کاربران {#users}

کاربران موجودیت‌هایی (یعنی قراردادهای هوشمند) هستند که برای انجام اقدامات خاص به اطلاعات خارج از بلاک چین نیاز دارند. گردش کار اصلی یک سرویس اوراکل با ارسال درخواست داده توسط کاربر به قرارداد اوراکل شروع می‌شود. درخواست‌های داده معمولاً به برخی یا همه سؤالات زیر پاسخ می‌دهند:

1. گره‌های خارج از زنجیره می‌توانند برای اطلاعات درخواستی از چه منابعی استفاده کنند؟

2. گزارشگران چگونه اطلاعات را از منابع داده پردازش می‌کنند و نقاط داده مفید را استخراج می‌کنند؟

3. چه تعداد گره یا نود اوراکل می‌توانند در بازیابی داده‌ها شرکت کنند؟

4. چگونه باید مغایرت‌های گزارش‌های اوراکل را مدیریت کرد؟

5. چه روشی باید در فیلتر کردن مطالب ارسالی و تجمیع گزارش‌ها در یک مقدار واحد اجرا شود؟

### قرارداد اوراکل {#oracle-contract}

قرارداد اوراکل جزء زنجیره‌ای برای سرویس اوراکل است. به درخواست‌های داده از قراردادهای دیگر گوش می‌دهد، پرس و جوهای داده را به گره‌های اوراکل رله کرده و داده‌های برگشتی را به قراردادهای کلاینت پخش می‌کند. این قرارداد همچنین ممکن است برخی از محاسبات را روی نقاط داده برگشتی انجام دهد تا یک مقدار مجموع برای ارسال به قرارداد درخواست کننده ایجاد کند.

قرارداد اوراکل برخی از توابع را نشان می‌دهد که قراردادهای کلاینت هنگام درخواست داده آنها را فراخوانی می‌کنند. پس از دریافت یک درخواست جدید، قرارداد هوشمند یک [رویداد گزارش یا همان ایونت لاگ](/developers/docs/smart-contracts/anatomy/#events-and-logs) را با جزئیات درخواست داده ارسال می‌کند. این مورد به گره‌های خارج از زنجیره مشترک در گزارش (معمولاً از چیزی مانند دستور JSON-RPC `eth_subscribe` استفاده می‌کند)، که به بازیابی داده‌های تعریف‌شده در رویداد لاگ می‌پردازند.

در زیر یک [نمونه قرارداد اوراکل](https://medium.com/@pedrodc/implementing-a-blockchain-oracle-on-ethereum-cedc7e26b49e) توسط پدرو کاستا آمده است. این یک سرویس اوراکل ساده است که می‌تواند در صورت درخواست سایر قراردادهای هوشمند، APIهای خارج از زنجیره را جستجو کند و اطلاعات درخواستی را در زنجیره بلوکی ذخیره کند:

```solidity
pragma solidity >=0.4.21 <0.6.0;

contract Oracle {
  Request[] requests; //list of requests made to the contract
  uint currentId = 0; //increasing request id
  uint minQuorum = 2; //minimum number of responses to receive before declaring final result
  uint totalOracleCount = 3; // Hardcoded oracle count

  // defines a general api request
  struct Request {
    uint id;                            //request id
    string urlToQuery;                  //API url
    string attributeToFetch;            //json attribute (key) to retrieve in the response
    string agreedValue;                 //value from key
    mapping(uint => string) answers;     //answers provided by the oracles
    mapping(address => uint) quorum;    //oracles which will query the answer (1=oracle hasn't voted, 2=oracle has voted)
  }

  //event that triggers oracle outside of the blockchain
  event NewRequest (
    uint id,
    string urlToQuery,
    string attributeToFetch
  );

  //triggered when there's a consensus on the final result
  event UpdatedRequest (
    uint id,
    string urlToQuery,
    string attributeToFetch,
    string agreedValue
  );

  function createRequest (
    string memory _urlToQuery,
    string memory _attributeToFetch
  )
  public
  {
    uint length = requests.push(Request(currentId, _urlToQuery, _attributeToFetch, ""));
    Request storage r = requests[length-1];

    // Hardcoded oracles address
    r.quorum[address(0x6c2339b46F41a06f09CA0051ddAD54D1e582bA77)] = 1;
    r.quorum[address(0xb5346CF224c02186606e5f89EACC21eC25398077)] = 1;
    r.quorum[address(0xa2997F1CA363D11a0a35bB1Ac0Ff7849bc13e914)] = 1;

    // launch an event to be detected by oracle outside of blockchain
    emit NewRequest (
      currentId,
      _urlToQuery,
      _attributeToFetch
    );

    // increase request id
    currentId++;
  }

  //called by the oracle to record its answer
  function updateRequest (
    uint _id,
    string memory _valueRetrieved
  ) public {

    Request storage currRequest = requests[_id];

    //check if oracle is in the list of trusted oracles
    //and if the oracle hasn't voted yet
    if(currRequest.quorum[address(msg.sender)] == 1){

      //marking that this address has voted
      currRequest.quorum[msg.sender] = 2;

      //iterate through "array" of answers until a position if free and save the retrieved value
      uint tmpI = 0;
      bool found = false;
      while(!found) {
        //find first empty slot
        if(bytes(currRequest.answers[tmpI]).length == 0){
          found = true;
          currRequest.answers[tmpI] = _valueRetrieved;
        }
        tmpI++;
      }

      uint currentQuorum = 0;

      //iterate through oracle list and check if enough oracles(minimum quorum)
      //have voted the same answer as the current one
      for(uint i = 0; i < totalOracleCount; i++){
        bytes memory a = bytes(currRequest.answers[i]);
        bytes memory b = bytes(_valueRetrieved);

        if(keccak256(a) == keccak256(b)){
          currentQuorum++;
          if(currentQuorum >= minQuorum){
            currRequest.agreedValue = _valueRetrieved;
            emit UpdatedRequest (
              currRequest.id,
              currRequest.urlToQuery,
              currRequest.attributeToFetch,
              currRequest.agreedValue
            );
          }
        }
      }
    }
  }
}
```

### گره یا نودهای اوراکل {#oracle-nodes}

گره یا نود اوراکل جزء خارج از زنجیره سرویس اوراکل است. این اطلاعات را از منابع خارجی، مانند APIهای میزبانی شده در سرورهای شخص ثالث استخراج می‌کند و آن را برای مصرف قراردادهای هوشمند در زنجیره قرار می‌دهد. گره‌ یا نودهای اوراکل به رویدادهای قرارداد اوراکل روی زنجیره گوش می‌دهند و به تکمیل کار توضیح داده شده در گزارش ادامه می‌دهند.

یک کار رایج برای نودهای اوراکل ارسال یک درخواست [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) به یک سرویس API، تجزیه پاسخ برای استخراج داده‌های مرتبط است. فرمت کردن به یک خروجی قابل خواندن از طریق بلاک چین و ارسال آن در زنجیره (آنچین) با گنجاندن آن در تراکنش به قرارداد اوراکل است. همچنین ممکن است به گره یا نود اوراکل نیاز باشد تا اعتبار و یکپارچگی اطلاعات ارسالی را با استفاده از «اثبات اصالت» تأیید کند، که بعداً بررسی خواهیم کرد.

اوراکل‌های محاسباتی همچنین به گره‌ یا نودهای خارج از زنجیره برای انجام وظایف محاسباتی متکی هستند که با توجه به هزینه‌های گس و محدودیت اندازه بلوک، اجرای آنها در زنجیره غیرعملی است. به عنوان مثال، گره یا نود اوراکل ممکن است وظیفه تولید یک رقم تصادفی قابل تأیید را داشته باشد (به عنوان مثال، برای بازی‌های مبتنی بر بلاک‌چین).

## الگوهای طراحی اوراکل {#oracle-design-patterns}

اوراکل‌ها انواع مختلفی دارند، از جمله _خواندن فوری_، _publish-subscribe_، و _Request-Response_، که دو مورد اخیر محبوب‌ترین در میان قراردادهای هوشمند اتریوم هستند. در اینجا به طور خلاصه مدل‌های انتشار-اشتراک و درخواست-پاسخ را توضیح می‌دهیم.

### اوراکل‌های انتشار و اشتراک {#publish-subscribe-oracles}

این نوع اوراکل یک "فید داده" را در معرض دید قرار می‌دهد که سایر قراردادها می‌توانند به طور منظم برای اطلاعات بخوانند. انتظار می‌رود که داده‌ها در این مورد به طور مکرر تغییر کنند، بنابراین قراردادهای مشتری باید برای به‌روزرسانی داده‌های ذخیره‌سازی اوراکل گوش (listen) (نوعی از اصطلاحات در خصوص برنامه نویسی) دهند. به عنوان مثال اوراکلی است که آخرین اطلاعات قیمت ETH-USD را در اختیار کاربران قرار می‌دهد.

### اوراکل‌های درخواست-پاسخ {#request-response-oracles}

تنظیم درخواست-پاسخ به قرارداد مشتری یا کلاینت اجازه می‌دهد تا داده‌های دلخواه را غیر از آنچه توسط یک اوراکل انتشار-اشتراک ارائه می‌شود، درخواست کند. اوراکل‌های درخواست-پاسخ زمانی ایده‌آل هستند که مجموعه داده آن‌قدر بزرگ است که نمی‌توان آن‌ها را در فضای ذخیره‌سازی قرارداد هوشمند ذخیره کرد و/یا کاربران تنها به بخش کوچکی از داده‌ها در هر مقطع زمانی نیاز خواهند داشت.

اگرچه پیچیده‌تر از مدل‌های انتشار-اشتراک است، اما اوراکل‌های درخواست پاسخ اساساً همان چیزی است که در بخش قبل توضیح دادیم. اوراکل دارای یک جزء روی زنجیره خواهد بود که درخواست داده را دریافت کرده و آن را برای پردازش به یک گره یا نود خارج از زنجیره ارسال می‌کند.

کاربرانی که درخواست‌های داده را آغاز می‌کنند باید هزینه بازیابی اطلاعات از منبع خارج از زنجیره را پوشش دهند. همچنین قرارداد کلاینت باید وجوهی را برای پوشش هزینه‌های گس متحمل شده توسط قرارداد اوراکل در بازگرداندن پاسخ از طریق تابع کالبک به تماس مشخص شده در درخواست فراهم کند.

## اوراکل‌های متمرکز در مقابل غیرمتمرکز {#types-of-oracles}

### اوراکل‌های متمرکز {#centralized-oracles}

یک اوراکل متمرکز توسط یک نهاد واحد کنترل می‌شود که مسئول جمع‌آوری اطلاعات خارج از زنجیره و به روز رسانی داده‌های قرارداد اوراکل در صورت درخواست است. اوراکل‌های متمرکز کارآمد هستند زیرا بر یک منبع حقیقی تکیه دارند. آنها ممکن است در مواردی که مجموعه داده‌های اختصاصی مستقیماً توسط مالک با امضای پذیرفته شده منتشر می‌شود بهتر عمل کنند. با این حال، آنها جنبه‌های منفی نیز دارند:

#### صحت کم را تضمین می‌کند {#low-correctness-guarantees}

با اوراکل‌های متمرکز، هیچ راهی برای تأیید صحت یا عدم صحت اطلاعات ارائه شده وجود ندارد. حتی ارائه دهندگان "معتبر" می‌توانند سرکش یا هک شوند. اگر اوراکل فاسد شود، قراردادهای هوشمند بر اساس داده‌های نامناسب اجرا می‌شوند.

#### در دسترس بودن ضعیف {#poor-availability}

اوراکل‌های متمرکز تضمین نمی‌کنند که همیشه داده‌های خارج از زنجیره را در اختیار سایر قراردادهای هوشمند قرار دهند. اگر ارائه‌دهنده تصمیم بگیرد سرویس را خاموش کند یا هکری مؤلفه خارج از زنجیره اوراکل را ربود، قرارداد هوشمند شما در معرض خطر حمله انکار سرویس (DoS) قرار دارد.

#### سازگاری انگیزشی ضعیف {#poor-incentive-compatibility}

اوراکل‌های متمرکز اغلب با انگیزه‌های ضعیفی طراحی شده یا برای ارائه‌دهنده داده برای ارسال اطلاعات دقیق/بدون تغییر وجود ندارند. پرداخت به اوراکل برای صحت، صداقت را تضمین نمی‌کند. این مشکل با افزایش مقدار ارزش کنترل شده توسط قراردادهای هوشمند بزرگتر می‌شود.

### اوراکل‌های غیرمتمرکز {#decentralized-oracles}

اوراکل‌های غیرمتمرکز برای غلبه بر محدودیت‌های اوراکل‌های متمرکز با حذف نقاط شکست منفرد طراحی شده‌اند. یک سرویس غیرمتمرکز اوراکل شامل چندین شرکت‌کننده در یک شبکه همتا به همتا است که قبل از ارسال آن به یک قرارداد هوشمند، روی داده‌های خارج از زنجیره یا آفچین اتفاق نظر دارند.

یک اوراکل غیرمتمرکز (در حالت ایده آل) باید بدون مجوز، بدون اعتماد و عاری از اداره یک حزب مرکزی باشد؛ در واقعیت، تمرکززدایی در میان اوراکل ها در یک طیف است. شبکه‌های اوراکل نیمه غیرمتمرکز وجود دارد که هر کسی می‌تواند در آن شرکت کند، اما با یک «مالک» که گره‌ها را بر اساس عملکرد تاریخی تأیید و حذف می‌کند باشد. شبکه‌های اوراکل کاملاً غیرمتمرکز نیز وجود دارند: این شبکه‌ها معمولاً به‌عنوان زنجیره‌های بلوکی یا همان بلاکچین مستقل اجرا می‌شوند و مکانیزم‌های اجماع مشخصی برای هماهنگ کردن گره‌ها و مجازات رفتارهای نادرست دارند.

استفاده از اوراکل‌های غیرمتمرکز دارای مزایای زیر است:

### صحت بالا را تضمین می‌کند {#high-correctness-guarantees}

اوراکل‌های غیرمتمرکز تلاش می‌کنند تا با استفاده از رویکردهای مختلف به صحت داده‌ها دست یابند. این مورد شامل استفاده از شواهدی است که صحت و یکپارچگی اطلاعات بازگردانده شده را تأیید می‌کند و لازم است چندین نهاد به طور جمعی در مورد اعتبار داده‌های خارج از زنجیره به توافق برسند.

#### اثبات اصالت {#authenticity-proofs}

اثبات اصالت مکانیزم‌های رمزنگاری هستند که امکان تأیید مستقل اطلاعات بازیابی شده از منابع خارجی را فراهم می‌کنند. این شواهد می‌توانند منبع اطلاعات را تایید و تغییرات احتمالی داده‌ها را پس از بازیابی شناسایی کنند.

نمونه‌هایی از اثبات اصالت عبارتند از:

**اثبات امنیت لایه انتقال (TLS)**: گره‌های اوراکل اغلب داده‌ها را با استفاده از یک اتصال HTTP ایمن بر اساس پروتکل امنیت لایه انتقال (TLS) از منابع خارجی بازیابی می‌کنند. برخی از اوراکل‌های غیرمتمرکز برای تأیید جلسات TLS (یعنی تأیید تبادل اطلاعات بین یک گره و یک سرور خاص) و تأیید عدم تغییر محتویات جلسه، از اثبات‌های اعتبار یا اصالت استفاده می‌کنند.

**تأییدات محیط اجرای مورد اعتماد (TEE)**: یک [محیط اجرای مورد اعتماد](https://en.wikipedia.org/wiki/Trusted_execution_environment) (TEE) یک محیط محاسباتی سندباکس شده است که از فرآیندهای عملیاتی سیستم میزبان خود جدا شده است. TEEها اطمینان حاصل می‌کنند که هر کد برنامه یا داده‌ای که در محیط محاسباتی ذخیره/استفاده می‌شود، یکپارچگی، محرمانه بودن و تغییرناپذیری را حفظ می‌کند. همچنین کاربران می‌توانند یک گواهی برای اثبات اینکه یک نمونه برنامه در محیط اجرای مورد اعتماد اجرا می‌شود، ایجاد کنند.

کلاس‌های خاصی از اوراکل‌های غیرمتمرکز به اپراتورهای گره اوراکل برای ارائه گواهی TEE نیاز دارند. این مورد به کاربر تأیید می‌کند که اپراتور گره نمونه‌ای از سرویس گیرنده اوراکل را در یک محیط اجرای مطمئن اجرا می‌کند. TEEها از تغییر یا خواندن کد و داده‌های برنامه توسط فرآیندهای خارجی جلوگیری می‌کنند، از این رو، این گواهی‌ها ثابت می‌کنند که گره اوراکل اطلاعات را دست نخورده و محرمانه نگه داشته است.

#### اعتبارسنجی مبتنی بر اجماع اطلاعات {#consensus-based-validation-of-information}

اوراکل‌های متمرکز هنگام ارائه داده‌ها به قراردادهای هوشمند به یک منبع حقیقت تکیه می‌کنند که امکان انتشار اطلاعات نادرست وجود دارد. اوراکل‌های غیرمتمرکز این مشکل را با تکیه بر چندین گره اوراکل برای جستجوی اطلاعات خارج از زنجیره حل می‌کنند. با مقایسه داده‌های چند منبع، اوراکل‌های غیرمتمرکز خطر انتقال اطلاعات نامعتبر به قراردادهای زنجیره‌ای را کاهش می‌دهند.

با این حال، اوراکل‌های غیرمتمرکز باید با اختلافات در اطلاعات بازیابی شده از چندین منبع خارج از زنجیره مقابله کنند. برای به حداقل رساندن تفاوت‌ها در اطلاعات و اطمینان از اینکه داده‌های ارسال شده به قرارداد اوراکل منعکس‌کننده نظر جمعی گره‌های اوراکل است، اوراکل‌های غیرمتمرکز از مکانیزم‌های زیر استفاده می‌کنند:

##### رای دادن/استیک کردن در مورد صحت داده‌ها

برخی از شبکه‌های اوراکل غیرمتمرکز از شرکت‌کنندگان می‌خواهند که در صحت پاسخ‌های پرسش‌های داده رای دهند یا در مورد صحت پاسخ‌ها استیک کنند (به عنوان مثال، "چه کسی در انتخابات 2020 ایالات متحده پیروز شد؟") با استفاده از توکن بومی شبکه. سپس یک پروتکل تجمیع آرا و سهام را جمع می‌کند و پاسخی را که اکثریت پشتیبانی می‌کند به عنوان پاسخ معتبر می‌گیرد.

گره‌هایی که پاسخ آنها از پاسخ اکثریت منحرف می‌شود، با توزیع توکن‌هایشان به دیگرانی که مقادیر صحیح‌تری ارائه می‌دهند، جریمه می‌شوند. اجبار گره‌ یا نودها برای ایجاد پیوند قبل از ارائه داده‌ها، انگیزه پاسخ‌های صادقانه را فراهم می‌کند، زیرا فرض می‌شود که آنها افراد اقتصادی منطقی هستند که قصد دارند بازده را به حداکثر برسانند.

استیک/رای‌گیری همچنین از اوراکل‌های غیرمتمرکز در برابر [حملات سبیل](/glossary/#sybil-attack) محافظت می‌کند که در آن عوامل مخرب چندین هویت را برای بازی با سیستم اجماع ایجاد می‌کنند. با این حال، استیک نمی‌تواند از "بارگذاری رایگان" (گره‌های اوراکل که اطلاعات را از دیگران کپی می‌کنند) و "اعتبارسنجی تنبل" (گره‌های اوراکل اکثریت را بدون تأیید اطلاعات خود دنبال می‌کنند) جلوگیری کند.

##### مکانیزم‌های نقطه هدف

[نقطه هدف](https://en.wikipedia.org/wiki/Focal_point_(game_theory)) یک مفهوم تئوری است که فرض می‌کند چندین موجودیت همیشه به طور پیش‌فرض به یک راه‌حل مشترک برای یک مشکل در عدم وجود هرگونه ارتباط می‌رسند. مکانیسم‌های شلینگ پوینت (Schelling-point) اغلب در شبکه‌های اوراکل غیرمتمرکز استفاده می‌شوند تا گره‌ یا نودها را قادر می‌سازد در مورد پاسخ به درخواست‌های داده به توافق برسند.

ایده اولیه برای این [SchellingCoin](https://blog.ethereum.org/2014/03/28/schellingcoin-a-minimal-trust-universal-data-feed/) بود، فید داده پیشنهادی که در آن شرکت‌کنندگان پاسخ‌هایی را به سؤالات «اسکالر» (سوالاتی که پاسخ‌های آن‌ها با بزرگی توصیف می‌شود، به‌عنوان مثال، «قیمت اتریوم چقدر است؟») همراه با سپرده ارسال می‌کنند. کاربرانی که مقادیری را بین 25 و 75 [درصد](https://en.wikipedia.org/wiki/Percentile) ارائه می‌کنند، پاداش می‌گیرند، در حالی که آن‌هایی که مقادیرشان تا حد زیادی از مقدار متوسط ​​انحراف دارد، جریمه می‌شوند.

در حالی که SchellingCoin امروزه وجود ندارد، تعدادی اوراکل غیرمتمرکز—به ویژه [پروتکل سازندگان اوراکل‌ها](https://docs.makerdao.com/smart-contract-modules/oracle-module)—از مکانیزم نقطه هدف برای بهبود دقت داده‌های اوراکل استفاده می‌کردند. هر Maker Oracle متشکل از یک شبکه P2P خارج از زنجیره از گره‌ها ("relayers" و "feed") است که قیمت‌های بازار را برای دارایی‌های وثیقه ارسال کرده و یک قرارداد "Medianizer" درون زنجیره‌ای که میانگین تمام ارزش‌های ارائه‌شده را محاسبه می‌کند. پس از پایان دوره تاخیر مشخص شده، این مقدار متوسط ​​به قیمت مرجع جدید برای دارایی مرتبط تبدیل می‌شود.

نمونه‌های دیگر اوراکل‌هایی که از مکانیزم‌های نقطه هدف استفاده می‌کنند عبارتند از [گزارش‌دهی خارج از زنجیره چین لینک](https://docs.chain.link/docs/off-chain-reporting/) و [Witnet](https://witnet.io/). در هر دو سیستم، پاسخ‌های گره‌ یا نودهای اوراکل در شبکه همتا به همتا در یک مقدار مجموع، مانند میانگین یا میانه، تجمیع می‌شوند. گره یا نود‌ها با توجه به میزانی که پاسخ هایشان با مقدار کل همسو یا انحراف دارد، پاداش یا مجازات می‌شوند.

مکانیسم‌های شلینگ پوینت (Schelling point) جذاب هستند زیرا ردپای روی زنجیره را به حداقل می‌رسانند (فقط یک تراکنش باید ارسال شود) در حالی که تمرکززدایی را تضمین می‌کند. مورد دوم امکان‌پذیر است زیرا گره‌ها باید قبل از وارد شدن به الگوریتمی که مقدار میانگین/میانگین را تولید می‌کند، در لیست پاسخ‌های ارسالی امضا کنند.

### در دسترس بودن {#availability}

خدمات غیرمتمرکز اوراکل در دسترس بودن بالای داده‌های خارج از زنجیره را برای قراردادهای هوشمند تضمین می‌کند. این امر با غیرمتمرکز کردن منبع اطلاعات خارج از زنجیره و گره یا نودهای مسئول انتقال اطلاعات در زنجیره به دست می‌آید.

این امر تحمل خطا را تضمین می‌کند زیرا قرارداد اوراکل می‌تواند به چندین گره یا نود (که همچنین به چندین منبع داده متکی هستند) برای اجرای پرس‌وجو از قراردادهای دیگر متکی باشد. تمرکززدایی در سطح منبع _و_ گره-اپراتور بسیار مهم است—شبکه ای از گره‌های اوراکل که اطلاعات بازیابی شده از یک منبع را ارائه می‌دهند، با مشکل مشابه یک اوراکل متمرکز مواجه خواهند شد.

همچنین برای اوراکل‌های مبتنی بر سهام می‌تواند اپراتورهای گره‌ یا نودی را که نمی‌توانند به سرعت به درخواست‌های داده پاسخ دهند، کاهش دهند. این به طور قابل توجهی گره یا نود‌های اوراکل را برای سرمایه‌گذاری در زیرساخت‌های مقاوم در برابر خطا و ارائه به موقع داده‌ها تشویق می‌کند.

### سازگاری انگیزشی خوب {#good-incentive-compatibility}

اوراکل‌های غیرمتمرکز طرح‌های تشویقی مختلفی را برای جلوگیری از رفتار [بیزانس](https://en.wikipedia.org/wiki/Byzantine_fault) در میان گره‌های اوراکل اجرا می‌کنند. به طور خاص، آنها به _قابلیت انتساب_ و _پاسخگویی_ دست می‌یابند:

1. گره یا نودهای اوراکل غیرمتمرکز اغلب برای امضای داده‌هایی که در پاسخ به درخواست‌های داده ارائه می‌کنند مورد نیاز است. این اطلاعات به ارزیابی عملکرد تاریخی گره یا نودهای اوراکل کمک می‌کند، به طوری که کاربران می‌توانند هنگام درخواست داده، گره یا نودهای اوراکل غیرقابل اعتماد را فیلتر کنند. یک مثال [سیستم شهرت الگوریتمی](https://docs.witnet.io/intro/about/architecture#algorithmic-reputation-system) Witnet است.

2. اوراکل‌های غیرمتمرکز - همانطور که قبلاً توضیح داده شد - ممکن است به گره‌ یا نودهایی نیاز داشته باشند که در مورد اطمینان خود نسبت به صحت داده‌هایی که ارسال می‌کنند، سهمی داشته باشند. در صورت بررسی ادعا، این سهام می‌تواند همراه با پاداش برای خدمات صادقانه بازگردانده شود. اما در صورت نادرست بودن اطلاعات نیز می‌توان آن را کاهش داد، که مقداری از پاسخگویی را فراهم می‌کند.

## کاربردهای اوراکل در قراردادهای هوشمند {#applications-of-oracles-in-smart-contracts}

موارد زیر موارد استفاده رایج برای اوراکل‌ها در اتریوم است:

### بازیابی اطلاعات مالی {#retrieving-financial-data}

برنامه‌های [مالی غیرمتمرکز](/defi/) (DeFi) امکان وام‌دهی، استقراض و معامله دارایی‌ها را به صورت همتا به همتا فراهم می‌کنند. این مورد اغلب مستلزم دریافت اطلاعات مالی مختلف، از جمله داده‌های نرخ مبادله (برای محاسبه ارزش فیات ارزهای دیجیتال یا مقایسه قیمت‌های توکن) و داده‌های بازار سرمایه (برای محاسبه ارزش دارایی‌های توکن‌شده، مانند طلا یا دلار آمریکا) است.

به عنوان مثال، یک پروتکل وام دهی دیفای، نیاز به استعلام قیمت‌های فعلی بازار برای دارایی‌ها (به عنوان مثال، اتر) دارد که به عنوان وثیقه سپرده شده است. این به قرارداد اجازه می‌دهد تا ارزش دارایی‌های وثیقه را تعیین کند و تعیین کند که چقدر می‌تواند از سیستم وام بگیرد.

«اوراکل‌های قیمت» محبوب (که معمولاً به آن‌ها گفته می‌شود) در دیفای شامل فیدهای قیمت زنجیره‌ای، پروتکل ترکیبی [فید قیمت باز](https://compound.finance/docs/prices) <a> یونی سواپ است. href="https://docs.uniswap.org/contracts/v2/concepts/core-concepts/oracles">قیمت‌های میانگین وزن‌دار زمانی (TWAP)</a> و [میکر اوراکل](https://docs.makerdao.com/smart-contract-modules/oracle-module).

سازندگان باید قبل از ادغام آنها در پروژه خود، اخطارهایی را که با این اوراکل‌های قیمت همراه است، درک کنند. این [مقاله](https://blog.openzeppelin.com/secure-smart-contract-guidelines-the-dangers-of-price-oracles/) تجزیه و تحلیل دقیقی از مواردی که باید در برنامه ریزی برای استفاده از هر یک از اوراکل‌های قیمت ذکر شده در نظر بگیرید ارائه می‌دهد.

در زیر مثالی از نحوه بازیابی آخرین قیمت اتر در قرارداد هوشمند خود با استفاده از فید قیمت زنجیره‌ای آورده شده است:

```solidity
pragma solidity ^0.6.7;

import "@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

contract PriceConsumerV3 {

    AggregatorV3Interface internal priceFeed;

    /**
     * Network: Kovan
     * Aggregator: ETH/USD
     * Address: 0x9326BFA02ADD2366b30bacB125260Af641031331
     */
    constructor() public {
        priceFeed = AggregatorV3Interface(0x9326BFA02ADD2366b30bacB125260Af641031331);
    }

    /**
     * Returns the latest price
     */
    function getLatestPrice() public view returns (int) {
        (
            uint80 roundID,
            int price,
            uint startedAt,
            uint timeStamp,
            uint80 answeredInRound
        ) = priceFeed.latestRoundData();
        return price;
    }
}
```

### ایجاد تصادفی قابل تأیید {#generating-verifiable-randomness}

برخی از برنامه‌های بلاک‌چین، مانند بازی‌های مبتنی بر بلاک‌چین یا طرح‌های بخت‌آزمایی، به سطح بالایی از غیرقابل پیش‌بینی و تصادفی بودن نیاز دارند تا به طور مؤثر کار کنند. با این حال، اجرای قطعی بلاک‌چین‌ها تصادفی بودن را از بین می‌برد.

رویکرد اولیه استفاده از توابع رمزنگاری شبه تصادفی، مانند `بلاک هش` بود، اما اینها ممکن [توسط ماینرها](https://ethereum.stackexchange.com/questions/3140/risk-of-using-blockhash-other-miners-preventing-attack#:~:text=است%20که%20the%20miners%20can,to%20one%20of%20the%20players.) برای حل مشکل الگوریتم اثبات کار دستکاری شوند. همچنین، [تغییر به اثبات سهام](/roadmap/merge/) اتریوم به این معنی است که توسعه‌دهندگان دیگر نمی‌توانند برای تصادفی بودن روی زنجیره به `بلاک هش` اعتماد کنند. در عوض، [مکانیزم RANDAO](https://eth2book.info/altair/part2/building_blocks/randomness) بیکون چین یک منبع جایگزین برای تصادفی بودن فراهم می‌کند.

امکان تولید ارزش تصادفی خارج از زنجیره و ارسال آن در زنجیره وجود دارد، اما انجام این کار الزامات اعتماد بالایی را به کاربران تحمیل می‌کند. آنها باید باور داشته باشند که ارزش واقعی از طریق مکانیسم‌های غیرقابل پیش‌بینی ایجاد شده است و در حمل و نقل تغییر نکرده است.

اوراکل‌هایی که برای محاسبات خارج از زنجیره طراحی شده‌اند، این مشکل را با تولید ایمن نتایج تصادفی خارج از زنجیره که روی زنجیره پخش می‌کنند همراه با اثبات‌های رمزنگاری که غیرقابل پیش‌بینی بودن فرآیند را تأیید می‌کنند، حل می‌کنند. یک مثال [چین لینک VRF](https://docs.chain.link/docs/chainlink-vrf/) (عملکرد تصادفی قابل تأیید) است که یک تولید کننده اعداد تصادفی منصفانه و بدون دستکاری است. (RNG) برای ساخت قراردادهای هوشمند قابل اعتماد برای برنامه‌هایی که بر نتایج غیرقابل پیش‌بینی تکیه دارند مفید است. مثال دیگر [API3 QRNG](https://docs.api3.org/explore/qrng/) است که تولید اعداد تصادفی کوانتومی (QRNG) را ارائه می‌کند، یک روش عمومی وب 3 RNG مبتنی بر پدیده‌های کوانتومی با هدف ارائه شده توسط دانشگاه ملی استرالیا (ANU) است.

### به دست آوردن نتایج برای رویدادها {#getting-outcomes-for-events}

با اوراکل‌ها، ایجاد قراردادهای هوشمند که به رویدادهای دنیای واقعی پاسخ می‌دهند، آسان است. خدمات اوراکل با اجازه دادن به قراردادها برای اتصال به APIهای خارجی از طریق اجزای خارج از زنجیره و مصرف اطلاعات از آن منابع داده، این امکان را فراهم می‌کند. به عنوان مثال، برنامه پیش‌بینی که قبلاً ذکر شد ممکن است از اوراکل درخواست کند که نتایج انتخابات را از یک منبع معتبر خارج از زنجیره (مثلاً آسوشیتد پرس) بازگرداند.

استفاده از اوراکل‌ها برای بازیابی داده‌ها بر اساس نتایج دنیای واقعی، سایر موارد استفاده جدید را امکان‌پذیر می‌کند. به عنوان مثال، یک محصول بیمه غیرمتمرکز به اطلاعات دقیق در مورد آب و هوا، بلایا و غیره نیاز دارد تا به طور مؤثر کار کند.

### خودکارسازی قراردادهای هوشمند {#automating-smart-contracts}

قراردادهای هوشمند به طور خودکار اجرا نمی‌شوند. بلکه یک حساب متعلق به خارجی (EOA)، یا یک حساب قرارداد دیگر، باید عملکردهای مناسب را برای اجرای کد قرارداد راه اندازی کند. در بیشتر موارد، بخش عمده‌ای از وظایف قرارداد عمومی است و می‌تواند توسط EOA و سایر قراردادها مورد استناد قرار گیرد.

اما همچنین _عملکردهای خصوصی_ در قرارداد وجود دارد که برای دیگران غیرقابل دسترسی است؛ اما برای عملکرد کلی یک برنامه غیرمتمرکز بسیار مهم است. مثال‌ها عبارتند از یک تابع `mintERC721Token()` که به صورت دوره‌ای NFT‌های جدید را برای کاربران مینت می‌کند، تابعی برای اعطای پرداخت‌ها در بازار پیش‌بینی، یا تابعی برای باز کردن قفل توکن‌های استیک شده در یک دکس است.

توسعه دهندگان باید چنین عملکردهایی را در فواصل زمانی فعال کنند تا برنامه به خوبی اجرا شود. با این حال، این مورد ممکن است منجر به از دست دادن ساعات بیشتری در انجام کارهای روزمره برای توسعه دهندگان شود، به همین دلیل است که اجرای خودکار قراردادهای هوشمند جذاب است.

برخی از شبکه‌های اوراکل غیرمتمرکز خدمات اتوماسیون را ارائه می‌کنند که به گره‌های اوراکل خارج از زنجیره اجازه می‌دهد تا عملکردهای قرارداد هوشمند را بر اساس پارامترهای تعریف شده توسط کاربر فعال کنند. به طور معمول، این امر مستلزم «ثبت» قرارداد هدف با سرویس اوراکل، تأمین بودجه برای پرداخت به اپراتور اوراکل و مشخص کردن شرایط یا زمان‌های شروع قرارداد است.

[شبکه کیپر](https://chain.link/keepers) چین لینک گزینه‌هایی را برای قراردادهای هوشمند برای برون‌سپاری وظایف تعمیر و نگهداری منظم به روشی به حداقل رسیده و غیرمتمرکز ارائه می‌دهد. [داکیومنت کیپر](https://docs.chain.link/docs/chainlink-keepers/introduction/) را برای اطلاعات در مورد سازگار کردن قرارداد خود با کیپر و استفاده از سرویس Upkeep بخوانید.

## نحوه استفاده از اوراکل‌های بلاک چین {#use-blockchain-oracles}

چندین برنامه اوراکل وجود دارد که می‌توانید آنها را در برنامه اتریوم خود ادغام کنید:

**[چین لینک](https://chain.link/)** - _شبکه‌های غیرمتمرکز اوراکل چین لینک ارائه می‌کنند ورودی‌ها، خروجی‌ها و محاسبات ضد دستکاری برای پشتیبانی از قراردادهای هوشمند پیشرفته در هر بلاک چین را اعمال می‌کند._

**[کرونیکل](https://chroniclelabs.org/)** - _کرونیکل بر محدودیت‌های فعلی غلبه می‌کند انتقال داده‌ها در زنجیره با توسعه اوراکل‌های واقعا مقیاس پذیر، مقرون به صرفه، غیرمتمرکز و قابل تأیید را پیاده‌سازی می‌کند._

**[Witnet](https://witnet.io/)** - _ویت نت بدون مجوز است، اوراکل غیرمتمرکز و مقاوم در برابر سانسور به قراردادهای هوشمند کمک می‌کند تا با ضمانت‌های ارزی-اقتصادی قوی به رویدادهای دنیای واقعی واکنش نشان دهند._

**[UMA Oracle](https://uma.xyz)** - _اوراکل آپتیمیستیک UMA به قراردادهای هوشمند اجازه می‌دهد قراردادهایی برای دریافت سریع و دریافت هر نوع داده برای برنامه‌های مختلف، از جمله بیمه، مشتقات مالی و بازارهای پیش بینی انجام دهند._

**[تلور](https://tellor.io/)** - _تلور شفاف و پروتکل اوراکل بدون مجوز برای قرارداد هوشمند شما است تا به راحتی هر داده‌ای را هر زمان که به آن نیاز داشتید دریافت کنید._

**[پروتکل باند](https://bandprotocol.com/)** - _پروتکل باند یک پلتفرم اوراکل داده متقابل زنجیره‌ای که داده‌ها و APIهای دنیای واقعی را جمع‌آوری و به قراردادهای هوشمند متصل می‌کند._

**[Paralink](https://paralink.network/)** - _پارالینک یک برنامه منبع باز ارائه می‌کند و پلتفرم اوراکل غیرمتمرکز برای قراردادهای هوشمند در حال اجرا بر روی اتریوم و سایر بلاک چین‌های محبوب است._

**[شبکه Pyth](https://pyth.network/)** - _شبکه Pyth یک شبکه اوراکل مالی شخص اول که برای انتشار داده‌های مستمر دنیای واقعی روی زنجیره در محیطی مقاوم در برابر دستکاری، غیرمتمرکز و خودپایدار طراحی شده است._

**[API3 DAO](https://www.api3.org/)** - _API3 DAO در حال ارائه راه حل‌های اوراکل شخص اول است که شفافیت منبع، امنیت و مقیاس پذیری بیشتری را در یک راه حل غیرمتمرکز برای قراردادهای هوشمند ارائه می‌کند_

**[Supra](https://supra.com/)** - یک جعبه ابزار یکپارچه از راه‌حل‌های زنجیره‌ای متقابل که همه بلاک چین‌ها را به هم متصل می‌کند. (L1ها و L2ها) یا خصوصی (تشکیلات)، ارائه فیدهای غیرمتمرکز قیمت اوراکل که می‌تواند برای موارد استفاده در زنجیره و خارج از زنجیره استفاده شود.

## بیشتر بخوانید {#further-reading}

**مقالات**

- [اوراکل بلاک چین چیست؟](https://chain.link/education/blockchain-oracles) — _چین لینک_
- [اوراکل بلاک چین چیست؟](https://betterprogramming.pub/what-is-a-blockchain-oracle-f5ccab8dbd72) — _پاتریک کالینز_
- [اوراکل‌های غیرمتمرکز: مروری جامع](https://medium.com/fabric-ventures/decentralised-oracles-a-comprehensive-overview-d3168b9a8841) — _ژولین تیونارد_
- [اجرای اوراکل بلاک چین در اتریوم](https://medium.com/@pedrodc/implementing-a-blockchain-oracle-on-ethereum-cedc7e26b49e) - *پدرو کاستا*
- [چرا قراردادهای هوشمند نمی‌توانند تماس‌های API برقرار کنند؟](https://ethereum.stackexchange.com/questions/301/why-cant-contracts-make-api-calls) — _StackExchange_
- [چرا به اوراکل‌های غیرمتمرکز نیاز داریم](https://newsletter.banklesshq.com/p/why-we-need-decentralized-oracles) — _Bankless_
- [پس می‌خواهید از اوراکل قیمت استفاده کنید](https://samczsun.com/so-you-want-to-use-a-price-oracle/) — _samczsun_

**ویدیوها**

- [Oracles و گسترش ابزار بلاک چین](https://youtu.be/BVUZpWa8vpw) — _بخش مالی ریل ویژن_
- [تفاوت‌های اوراکل‌های شخص اول و شخص ثالث](https://blockchainoraclesummit.io/first-party-vs-third-party-oracles/) - _Blockchain Oracle Summit_

**آموزش‌ها**

- [نحوه واکشی قیمت فعلی اتریوم در سالیدیتی](https://blog.chain.link/fetch-current-crypto-price-data-solidity/) — _چین لینک_
- [مصرف داده‌های اوراکل](https://docs.chroniclelabs.org/Developers/tutorials/Remix) — _کرونیکل_

**نمونه پروژه‌‌ها**

- [پروژه شروع کامل چین لینک برای اتریوم در سالیدیتی](https://github.com/hackbg/chainlink-fullstack) — _HackBG_
