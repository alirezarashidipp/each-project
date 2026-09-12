# چک‌لیست جامع LangChain و LangGraph

این نقشه، موضوعات لازم برای طراحی، ساخت، ارزیابی و اجرای سامانه‌های مبتنی بر LangChain و LangGraph را با تمرکز بر Python پوشش می‌دهد. مباحث LangSmith، امنیت، RAG و زیرساخت نیز در جایی آمده‌اند که برای یک برنامهٔ واقعی لازم‌اند. اولویت‌ها پیشنهاد آموزشی‌اند؛ ترتیب مستندات رسمی یا الزام یکسان برای همهٔ پروژه‌ها نیستند.

راهنما: **O = موجود در فهرست اولیه**؛ **E = تکمیل و جزئیات همان موضوع**؛ **N = موضوع افزوده**. **P1 = پایه و ضروری**؛ **P2 = لازم برای کاربرد جدی و production**؛ **P3 = تخصصی یا وابسته به نیاز**. موجود بودن یک موضوع به معنی تسلط بر آن نیست؛ همهٔ خانه‌های مطالعه ابتدا خالی‌اند.

لایه‌ها: **LC** کتابخانهٔ LangChain؛ **LG** کتابخانهٔ LangGraph؛ **LS** ابزارها و سرویس‌های LangSmith؛ **INT** اتصال‌ها و بسته‌های جانبی؛ **ENG** تصمیم‌ها و شیوه‌های عمومی مهندسی. این تفکیک مانع از آن می‌شود که یک نیاز زیرساختی با قابلیت آمادهٔ کتابخانه اشتباه گرفته شود.

## 01. جایگاه ابزارها، نسخه‌ها و انتخاب معماری

لایه: LC / LG / LS. هدف: مشخص شود کدام لایه چه مسئولیتی دارد.

- [ ] N · P1 — **Model در برابر Agent و Harness**؛ مدل تولید می‌کند و harness چرخه، ابزارها و محدودیت‌ها را اداره می‌کند.
- [ ] N · P1 — **LangChain در برابر LangGraph**؛ انتخاب harness آماده یا کنترل مستقیم جریان اجرا.
- [ ] N · P1 — **Workflow در برابر Agent**؛ مسیر ازپیش‌تعریف‌شده، تصمیم مدل، و ترکیب این دو.
- [ ] N · P1 — **`create_agent` و چرخهٔ model → tools → model**؛ شرایط خروج و خروجی state.
- [ ] N · P1 — **ساختار بسته‌ها**؛ `langchain`، `langchain-core`، `langgraph`، بستهٔ provider و `langchain-text-splitters`.
- [ ] N · P1 — **مهاجرت و APIهای قدیمی**؛ `create_react_agent`، `AgentExecutor`، `LLMChain` و جایگاه `langchain-classic`.
- [ ] N · P2 — **Version pinning و سازگاری dependencyها**؛ تطبیق راهنما، API reference و نسخهٔ نصب‌شده.
- [ ] N · P2 — **کیفیت و نگهداری integrationها**؛ رسمی یا community بودن، آزمون‌ها و وضعیت maintenance.
- [ ] N · P1 — **مرز کتابخانه و پلتفرم**؛ استفاده از OSS الزاماً نیازمند LangSmith Deployment نیست.
- [ ] N · P3 — **برابری نداشتن Python و JavaScript/TypeScript**؛ نام API و زمان عرضه را مستقل بررسی کنید.

منابع: [LangChain overview](https://docs.langchain.com/oss/python/langchain/overview)، [LangGraph overview](https://docs.langchain.com/oss/python/langgraph/overview)، [LangChain v1 migration](https://docs.langchain.com/oss/python/migrate/langchain-v1).

## 02. مدل‌ها، پیام‌ها و قرارداد گفتگو

لایه: LC / INT. هدف: ورودی و خروجی واقعی مدل درست فهمیده شود.

- [ ] N · P1 — **`init_chat_model` و adapterهای provider**؛ تنظیم مدل و تفاوت پارامترهای پشتیبانی‌شده.
- [ ] N · P1 — **نقش پیام‌ها**؛ `SystemMessage`، `HumanMessage`، `AIMessage` و `ToolMessage`.
- [ ] N · P1 — **`content`، `text` و `content_blocks`**؛ خروجی مدل همیشه یک رشتهٔ ساده نیست.
- [ ] N · P1 — **شناسهٔ پیام و `tool_call_id`**؛ پیوند هر نتیجهٔ ابزار با درخواست درست.
- [ ] N · P1 — **ترتیب معتبر پیام‌ها**؛ حفظ جفت tool call/result هنگام trim، حذف، ذخیره و replay.
- [ ] N · P2 — **`usage_metadata` و `response_metadata`**؛ مصرف توکن، علت پایان و اطلاعات provider.
- [ ] N · P2 — **Model capability profiles**؛ تشخیص پشتیبانی از ابزار، structured output، تصویر و محدودیت context.
- [ ] N · P2 — **Multimodal messages**؛ تصویر، فایل، صوت و محدودیت قالب و اندازهٔ ورودی.
- [ ] N · P2 — **Client-side و provider-side tools**؛ مشخص کردن محل اجرای واقعی ابزار.
- [ ] N · P3 — **Reasoning blocks و citation blocks**؛ استفاده از خروجی ارائه‌شدهٔ provider بدون فرض دسترسی به reasoning داخلی.

منابع: [Models](https://docs.langchain.com/oss/python/langchain/models)، [Messages](https://docs.langchain.com/oss/python/langchain/messages)، [Tools](https://docs.langchain.com/oss/python/langchain/tools).

## 03. Runnable، LCEL و ترکیب اجزا

لایه: LC. هدف: قرارداد اجرای اجزا و جریان داده مستقل از agent شناخته شود.

- [ ] O · P1 — **`Runnable` و LCEL**؛ `invoke`، `ainvoke`، `batch` و `stream`.
- [ ] E · P1 — **`abatch` و `astream`**؛ تفاوت اجرای async با اجرای چند ورودی.
- [ ] N · P1 — **`RunnableSequence` و عملگر `|`**؛ تطبیق نوع خروجی هر مرحله با ورودی بعدی.
- [ ] N · P2 — **`RunnableParallel`**؛ چند محاسبهٔ مستقل روی یک ورودی و جمع‌آوری خروجی‌ها.
- [ ] N · P1 — **`RunnableLambda`، `RunnablePassthrough`، `assign` و `pick`**؛ تبدیل و حفظ داده.
- [ ] N · P2 — **`bind`، `with_config` و `RunnableConfig`**؛ پارامتر ثابت در برابر تنظیم اجرای هر درخواست.
- [ ] N · P2 — **`batch_as_completed` و `abatch_as_completed`**؛ ترتیب تکمیل، اتصال پاسخ به ورودی و خطای جزئی.
- [ ] E · P2 — **`max_concurrency` و propagation تنظیمات**؛ عبور tags، metadata و callbacks به اجراهای داخلی.
- [ ] E · P2 — **`with_retry` و `with_fallbacks`**؛ اعمال سیاست به کوچک‌ترین بخش مناسب.
- [ ] N · P2 — **محدودیت composability**؛ LCEL به‌تنهایی جایگزین persistence، state و چرخه‌های LangGraph نیست.

منابع: [Runnables reference](https://reference.langchain.com/python/langchain-core/runnables)، [LangGraph overview](https://docs.langchain.com/oss/python/langgraph/overview).

## 04. Prompt و Structured Output

لایه: LC / ENG. هدف: قرارداد خروجی قابل‌اعتبارسنجی و ورودی قابل‌مدیریت باشد.

- [ ] N · P1 — **`ChatPromptTemplate`، متغیرها و `MessagesPlaceholder`**؛ ترکیب دستور، مثال و تاریخچه.
- [ ] N · P2 — **Few-shot examples و example selection**؛ انتخاب مثال مرتبط به‌جای رشد دائمی prompt.
- [ ] O · P1 — **Structured output با Pydantic و schema**؛ نوع، فیلد لازم، enum و اعتبارسنجی.
- [ ] N · P1 — **`with_structured_output` در برابر `create_agent(response_format=...)`**؛ خروجی یک model call در برابر خروجی agent.
- [ ] N · P1 — **`ProviderStrategy` در برابر `ToolStrategy`**؛ اجرای قرارداد توسط provider یا از مسیر tool calling.
- [ ] N · P2 — **سازگاری ابزار و schema با provider**؛ پشتیبانی هم‌زمان tool calling و structured output را بررسی کنید.
- [ ] E · P1 — **`structured_response` و validation failure**؛ فرق پاسخ معتبر، پاسخ ناقص و خطای parse.
- [ ] E · P2 — **Multiple structured outputs و repair محدود**؛ سیاست خطا و جلوگیری از تکرار بی‌پایان.
- [ ] N · P1 — **Schema validity در برابر business correctness**؛ JSON معتبر می‌تواند از نظر کسب‌وکار غلط باشد.
- [ ] N · P2 — **Output parsers**؛ کاربرد `StrOutputParser` و parserهای JSON در زنجیره‌های غیرagent.
- [ ] O · P2 — **Prompt versioning**؛ ثبت نسخهٔ prompt همراه مدل، ابزار، schema و نتایج evaluation.

منابع: [Structured output](https://docs.langchain.com/oss/python/langchain/structured-output)، [Chat prompt reference](https://reference.langchain.com/python/langchain-core/prompts/chat)، [Manage prompts](https://docs.langchain.com/langsmith/manage-prompts).

## 05. طراحی Tool و اجرای آن

لایه: LC / LG / ENG. هدف: مرز پیشنهاد مدل و اجرای برنامه روشن باشد.

- [ ] O · P1 — **Tool calling**؛ مدل نام و آرگومان پیشنهاد می‌کند؛ runtime یا provider مسئول اجراست.
- [ ] N · P1 — **`@tool`، type hints، docstring و `args_schema`**؛ قرارداد قابل‌فهم برای مدل و برنامه.
- [ ] N · P1 — **Tool selection و `tool_choice`**؛ محدودیت‌های provider و انتخاب اجباری یا خودکار.
- [ ] N · P2 — **چند tool call در یک پاسخ**؛ parallel tool calls، وابستگی و ترتیب اجرای مجاز.
- [ ] E · P1 — **`ToolRuntime`**؛ دسترسی به state، context، store، شناسهٔ call و stream writer.
- [ ] E · P1 — **Injected arguments**؛ اطلاعات trusted مانند user identity نباید از مدل گرفته شود.
- [ ] N · P2 — **Tool result در برابر artifact**؛ متن مناسب مدل، دادهٔ ساختاریافته و خروجی بزرگ برای برنامه.
- [ ] N · P2 — **تغییر state از ابزار با `Command`**؛ بازگرداندن update و پیام ابزار به‌شکل سازگار.
- [ ] E · P2 — **Tool error taxonomy**؛ خطای ورودی، business rejection، timeout و bug برنامه.
- [ ] E · P2 — **Error-as-result در برابر exception**؛ چه خطایی برای اصلاح به مدل برگردد و چه خطایی متوقف کند.
- [ ] O · P1 — **Least privilege و tool authorization**؛ مجوز واقعی در کد اجرایی بررسی شود.
- [ ] N · P2 — **`ToolNode` و `tools_condition` در graph سفارشی**؛ اتصال حلقهٔ ابزار به کنترل جریان.

منابع: [Tools](https://docs.langchain.com/oss/python/langchain/tools)، [Custom middleware](https://docs.langchain.com/oss/python/langchain/middleware/custom)، [Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api).

## 06. Middleware و Context Engineering

لایه: LC. هدف: رفتار agent بدون پراکندن کنترل‌ها در همهٔ ابزارها تنظیم شود.

- [ ] O · P1 — **معماری middleware در LangChain**؛ middleware به‌عنوان بخش سازندهٔ harness.
- [ ] E · P1 — **Lifecycle hooks**؛ `before_agent`، `before_model`، `after_model` و `after_agent`.
- [ ] E · P1 — **Wrap hooks**؛ `wrap_model_call` و `wrap_tool_call` و نسخه‌های async.
- [ ] N · P2 — **ترتیب middlewareها**؛ ترتیب ورود، خروج و تودرتویی wrapها در رفتار نهایی مؤثر است.
- [ ] E · P2 — **`ModelRequest` و override**؛ تغییر مدل، پیام‌ها، prompt یا tools برای همان call.
- [ ] O · P1 — **Dynamic prompts و context engineering**؛ انتخاب اطلاعات لازم برای هر مرحله.
- [ ] N · P1 — **Transient context در برابر persistent state update**؛ تغییر ورودی مدل الزاماً تغییر حافظه نیست.
- [ ] N · P2 — **Middleware-owned state**؛ تعریف state نزدیک به hook و ابزار مصرف‌کنندهٔ آن.
- [ ] E · P2 — **Dynamic tool filtering و tool selection**؛ کنترل مجموعهٔ ابزارهای قابل‌مشاهده در هر مرحله.
- [ ] O · P2 — **محدود کردن model/tool callها**؛ بودجهٔ run و thread و رفتار هنگام رسیدن به سقف.
- [ ] N · P2 — **Guardrail middleware**؛ PII، human approval، کنترل ورودی و کنترل خروجی.
- [ ] N · P3 — **Offloading و tool-output compression**؛ نگهداری دادهٔ بزرگ بیرون context و بازیابی انتخابی آن.

منابع: [Middleware overview](https://docs.langchain.com/oss/python/langchain/middleware/overview)، [Custom middleware](https://docs.langchain.com/oss/python/langchain/middleware/custom)، [Built-in middleware](https://docs.langchain.com/oss/python/langchain/middleware/built-in)، [Context engineering](https://docs.langchain.com/oss/python/langchain/context-engineering).

## 07. State، Context و Memory

لایه: LC / LG / ENG. هدف: محل و عمر هر داده آگاهانه انتخاب شود.

- [ ] O · P1 — **State، short-term memory و long-term memory**؛ state دادهٔ متغیر اجراست؛ حافظهٔ کوتاه‌مدت در state یک thread حفظ می‌شود و حافظهٔ بلندمدت میان threadها قابل‌استفاده است.
- [ ] N · P1 — **Runtime context و `context_schema`**؛ dependency و اطلاعات trusted یک invocation.
- [ ] O · P1 — **Checkpoint در برابر context management**؛ ذخیرهٔ وضعیت اجرا با انتخاب ورودی مدل متفاوت است.
- [ ] O · P1 — **`trim_messages` و نگه‌داشتن پیام‌های آخر**؛ با شمارش توکن و حفظ ترتیب معتبر پیام‌ها.
- [ ] O · P1 — **`SummarizationMiddleware`**؛ trigger، پیام‌های حفظ‌شده و مدل خلاصه‌ساز.
- [ ] O · P1 — **Summary + Recent Messages**؛ ترکیب سابقهٔ فشرده با جزئیات اخیر.
- [ ] O · P1 — **`RemoveMessage` و حذف از state**؛ نیاز به reducer مناسب پیام‌ها.
- [ ] O · P1 — **اطلاعات critical در structured state یا DB**؛ حقیقت معتبر کسب‌وکار فقط به summary وابسته نباشد.
- [ ] N · P2 — **حذف از state فعلی در برابر حذف از persistence**؛ checkpointهای قدیمی و traceها مستقل بررسی شوند.
- [ ] E · P2 — **Context window و token budget**؛ سهم system prompt، history، ابزار، اسناد و فضای پاسخ.
- [ ] N · P2 — **Context selection و ordering**؛ تازگی، ارتباط، حذف تکرار و جلوگیری از غلبهٔ دادهٔ کم‌ارزش.
- [ ] N · P2 — **Memory quality**؛ شناسایی summary drift، خاطرهٔ نادرست و تعارض اطلاعات جدید و قدیم.

منابع: [Short-term memory](https://docs.langchain.com/oss/python/langchain/short-term-memory)، [Context engineering](https://docs.langchain.com/oss/python/langchain/context-engineering)، [Persistence](https://docs.langchain.com/oss/python/langgraph/persistence).

## 08. Long-term Memory و Store

لایه: LG / LC / ENG. هدف: حافظهٔ بین گفتگوها دقیق، قابل‌حذف و محدود به کاربر درست باشد.

- [ ] E · P1 — **Checkpointer در برابر Store**؛ checkpoint اجرای thread با دادهٔ قابل‌بازیابی میان threadها فرق دارد.
- [ ] N · P1 — **Namespace و key**؛ جداسازی user، tenant، نوع حافظه و شناسهٔ رکورد.
- [ ] N · P2 — **عملیات store**؛ put، get، search و delete و نسخه‌های async.
- [ ] N · P2 — **Semantic search در Store**؛ embedding، فیلدهای indexشده و فیلتر metadata.
- [ ] N · P2 — **Semantic، episodic و procedural memory**؛ واقعیت، تجربه و شیوهٔ انجام کار.
- [ ] N · P2 — **Profile در برابر collection**؛ یک سند خلاصهٔ کاربر یا مجموعهٔ خاطرات مستقل.
- [ ] N · P2 — **Hot-path در برابر background memory writes**؛ اثر روی latency و تازگی حافظه.
- [ ] N · P2 — **سیاست نوشتن حافظه**؛ extraction، validation، deduplication و رفع تعارض.
- [ ] N · P2 — **عمر و منشأ خاطره**؛ timestamp، source، confidence و expiration به‌عنوان طراحی برنامه.
- [ ] N · P2 — **حذف و کنترل دسترسی حافظه**؛ بازخوانی و تغییر فقط در namespace مجاز.

منابع: [Long-term memory](https://docs.langchain.com/oss/python/langchain/long-term-memory)، [Stores](https://docs.langchain.com/oss/python/langgraph/stores)، [Memory concepts](https://docs.langchain.com/oss/python/concepts/memory).

## 09. LangGraph Graph API و State Schema

لایه: LG. هدف: قرارداد داده و lifecycle گراف درست طراحی شود.

- [ ] N · P1 — **`StateGraph`، node، edge، `START` و `END`**؛ ساخت و اجرای گراف پایه.
- [ ] N · P1 — **Build، compile و invoke**؛ چه تنظیماتی هنگام ساخت و چه داده‌ای هنگام اجرا داده می‌شود.
- [ ] O · P1 — **Custom state schema و reducerها**؛ نوع هر کانال و قانون اعمال update.
- [ ] E · P1 — **TypedDict، dataclass و Pydantic در Graph API**؛ validation مستندشدهٔ Pydantic برای ورودی نخستین node است؛ همهٔ updateها و خروجی graph خودکار validate نمی‌شوند.
- [ ] N · P1 — **Partial state updates**؛ هر node فقط تغییرات لازم را بازگرداند.
- [ ] N · P1 — **Overwrite در برابر accumulate**؛ رفتار پیش‌فرض فیلد و reducerهایی مانند جمع لیست.
- [ ] N · P1 — **`MessagesState` و `add_messages`**؛ ادغام پیام بر اساس ID و پشتیبانی از حذف.
- [ ] N · P2 — **Input، output و private schemas**؛ تفکیک قراردادها؛ private بودن به معنی حذف داده از stream نیست.
- [ ] N · P2 — **`Overwrite` و عبور کنترل‌شده از reducer**؛ جایگزینی صریح مقدار.
- [ ] N · P2 — **Reducer correctness**؛ اثر ترتیب updateها، تکرار، تعارض و mutation ناخواسته.
- [ ] N · P2 — **Node signature و runtime injection**؛ state، config و context هر کدام چه نقشی دارند.
- [ ] N · P2 — **Graph visualization و validation**؛ بررسی مسیرهای بدون پایان، unreachable و قرارداد خروجی.

منابع: [Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api)، [Use the Graph API](https://docs.langchain.com/oss/python/langgraph/use-graph-api)، [LangChain v1 migration](https://docs.langchain.com/oss/python/migrate/langchain-v1).

## 10. Routing، Parallelism و مدل اجرای گراف

لایه: LG. هدف: اجرای موازی و مسیرهای پویا قابل‌پیش‌بینی باشند.

- [ ] O · P1 — **Conditional edges و routing**؛ تصمیم rule-based یا مدل با خروجی محدود و معتبر.
- [ ] O · P1 — **`Command` برای update + routing**؛ استفاده از `update` و `goto` در یک نتیجه.
- [ ] N · P1 — **تفاوت `Command` با static edge**؛ `goto` مسیر ثابت موجود را خودکار حذف نمی‌کند.
- [ ] N · P1 — **`Send` و map-reduce پویا**؛ ایجاد تعداد متغیر کار با state مخصوص هر branch.
- [ ] O · P1 — **Fan-out / fan-in**؛ شاخه‌های مستقل و مرحلهٔ جمع‌بندی.
- [ ] N · P1 — **Barrier و join**؛ تفاوت edge از فهرست nodeها با چند edge مستقل.
- [ ] N · P2 — **Pregel و superstep**؛ مراحل plan، execute و update و مرز مشاهدهٔ تغییرات.
- [ ] N · P2 — **Concurrent writes به یک key**؛ reducer مناسب و خطای update هم‌زمان.
- [ ] N · P2 — **شاخه‌های با طول متفاوت و `defer=True`**؛ زمان درست اجرای aggregation.
- [ ] N · P1 — **Loops، stop condition و `recursion_limit`**؛ سقف superstep با سقف tool call یکی نیست.
- [ ] N · P2 — **Partial failure در شاخه‌ها**؛ سیاست حفظ نتیجهٔ موفق و بازیابی بخش شکست‌خورده.
- [ ] N · P3 — **Bounded fan-out**؛ کنترل بار provider، DB و ابزار هنگام افزایش تعداد کارها.

منابع: [Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api)، [Use the Graph API](https://docs.langchain.com/oss/python/langgraph/use-graph-api)، [Pregel runtime](https://docs.langchain.com/oss/python/langgraph/pregel).

## 11. Functional API و Subgraphs

لایه: LG. هدف: انتخاب سطح انتزاع و ترکیب workflowها آگاهانه باشد.

- [ ] N · P2 — **Graph API در برابر Functional API**؛ انتخاب گراف صریح یا کنترل جریان Python.
- [ ] N · P2 — **`@entrypoint` و `@task`**؛ مرز workflow، کار پایدار و نتیجهٔ قابل‌بازیابی.
- [ ] N · P2 — **`previous` و `entrypoint.final`**؛ تفکیک مقدار بازگشتی از مقدار ذخیره‌شده.
- [ ] N · P2 — **Determinism و task boundaries**؛ قرار دادن عملیات nondeterministic و side effect در مرز مناسب.
- [ ] O · P2 — **Subgraphs**؛ استفادهٔ مجدد، composition و تقسیم مسئولیت.
- [ ] E · P2 — **Shared state در برابر state mapping**؛ اشتراک keyها یا تبدیل ورودی و خروجی subgraph.
- [ ] N · P2 — **Per-invocation، per-thread و stateless subgraph**؛ انتخاب scope حافظه.
- [ ] N · P2 — **Checkpoint inheritance و namespace**؛ مسیر state زیرگراف در persistence والد.
- [ ] N · P2 — **Subgraph interrupts و streaming**؛ مشاهده و ازسرگیری در مرز والد و فرزند.
- [ ] N · P2 — **Parallel calls به همان subgraph با persistence از نوع per-thread**؛ جلوگیری از برخورد namespace؛ اجرای per-invocation می‌تواند مستقل باشد.

منابع: [Functional API](https://docs.langchain.com/oss/python/langgraph/functional-api)، [Subgraphs](https://docs.langchain.com/oss/python/langgraph/use-subgraphs).

## 12. Persistence، Durable Execution و بازیابی

لایه: LG / ENG. هدف: توقف فرایند به خراب شدن یا تکرار ناخواستهٔ عملیات منجر نشود.

- [ ] O · P1 — **Checkpointing، persistence و `thread_id`**؛ هویت ادامهٔ اجرای stateful.
- [ ] N · P1 — **`StateSnapshot` و checkpoint metadata**؛ values، next، tasks و اطلاعات lineage.
- [ ] N · P2 — **`checkpoint_id` و `checkpoint_ns`**؛ انتخاب نسخه و محدودهٔ state.
- [ ] N · P2 — **Checkpoint در برابر pending writes**؛ بازیابی خروجی nodeهای موفق در یک superstep ناقص.
- [ ] N · P2 — **Durability modes**؛ `sync`، `async` و `exit` و trade-off هزینه و دوام.
- [ ] O · P2 — **Production checkpointer مانند Postgres**؛ setup، connection pool و lifecycle اتصال.
- [ ] N · P2 — **In-memory و SQLite در برابر ذخیرهٔ production**؛ انتخاب بر اساس دوام و هم‌زمانی لازم.
- [ ] O · P1 — **Idempotency برای side effectها**؛ شناسهٔ عملیات و تشخیص انجام‌شدن قبلی در مقصد.
- [ ] N · P2 — **Crash window میان side effect و ثبت نتیجه**؛ checkpointer به‌تنهایی تضمین exactly-once خارجی نیست.
- [ ] N · P2 — **Serialization و دادهٔ قابل‌ذخیره**؛ objects پیچیده، secrets و تغییر قرارداد داده.
- [ ] O · P2 — **Long-running workflows**؛ restart، resume و ادامه پس از قطع فرایند.
- [ ] N · P2 — **Retention، backup و recovery drills**؛ آزمودن واقعی بازیابی، نه فقط نوشتن checkpoint.

منابع: [Persistence](https://docs.langchain.com/oss/python/langgraph/persistence)، [Checkpointers](https://docs.langchain.com/oss/python/langgraph/checkpointers)، [Functional API](https://docs.langchain.com/oss/python/langgraph/functional-api).

## 13. Human-in-the-loop، Replay و Time Travel

لایه: LG / LC / ENG. هدف: توقف، بررسی انسانی و اجرای دوباره اثر مشخص داشته باشند.

- [ ] O · P1 — **`interrupt()` و human-in-the-loop**؛ توقف برای approve، edit، reject یا دریافت اطلاعات.
- [ ] N · P1 — **`Command(resume=...)` با همان `thread_id`**؛ قرارداد ادامه‌دادن اجرای متوقف‌شده.
- [ ] N · P1 — **Restart شدن node هنگام resume**؛ کد عادی قبل از interrupt دوباره اجرا می‌شود؛ نتیجهٔ task تکمیل‌شده می‌تواند از checkpoint بازخوانی شود.
- [ ] N · P1 — **Interrupt به‌عنوان control flow**؛ آن را در `except` عمومی پنهان نکنید.
- [ ] N · P2 — **ترتیب چند interrupt و شناسهٔ آن‌ها**؛ نگاشت پاسخ‌ها، به‌ویژه در شاخه‌های موازی.
- [ ] N · P2 — **Serializable interrupt payload**؛ قرارداد دادهٔ نمایش‌داده‌شده به بررسی‌کننده و پاسخ او.
- [ ] N · P2 — **Debug breakpoints در برابر dynamic interrupts**؛ `interrupt_before/after` با approval کسب‌وکار یکی نیست.
- [ ] O · P2 — **Time travel و replay**؛ انتخاب checkpoint و بررسی مسیر اجرای جدید.
- [ ] N · P2 — **`get_state`، `get_state_history` و `update_state`**؛ inspection، fork و اثر reducerها.
- [ ] N · P2 — **`as_node` و مسیر بعدی اجرا**؛ به‌روزرسانی state می‌تواند scheduling بعدی را تغییر دهد.
- [ ] N · P1 — **Replay در برابر rollback خارجی**؛ اجرای دوباره پیام ارسال‌شده یا پرداخت انجام‌شده را پس نمی‌گیرد.
- [ ] N · P2 — **Replay در برابر recovery**؛ اجرای دوبارهٔ عمدی ممکن است مدل، API و interrupt را تکرار کند.

منابع: [Interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts)، [Time travel](https://docs.langchain.com/oss/python/langgraph/use-time-travel)، [Checkpointers](https://docs.langchain.com/oss/python/langgraph/checkpointers).

## 14. Reliability، خطا و پایان امن اجرا

لایه: LC / LG / ENG. هدف: خطاهای قابل‌بازیابی و خطاهای نهایی رفتار متفاوت داشته باشند.

- [ ] O · P1 — **Retry، timeout، fallback و rate limit**؛ سیاست مجزا برای مدل، ابزار و کل workflow.
- [ ] E · P2 — **Transient در برابر permanent error**؛ retry فقط برای خطاهایی که احتمال رفع دارند.
- [ ] E · P2 — **Exponential backoff، jitter و سقف تلاش**؛ کنترل فشار و زمان انتظار.
- [ ] N · P2 — **Retry amplification**؛ جمع‌شدن retryهای SDK، middleware، node و worker.
- [ ] E · P2 — **Timeout هر تلاش در برابر deadline کل run**؛ جلوگیری از تجاوز بودجهٔ کلی.
- [ ] N · P2 — **Error handler، recovery route و compensation**؛ مرحلهٔ بعد پس از پایان retryها.
- [ ] N · P2 — **Fallback compatibility**؛ سازگاری ابزار، schema، context و سیاست داده با مدل جایگزین.
- [ ] N · P2 — **Cancellation و cleanup**؛ بستن connection و تعیین تکلیف عملیات نیمه‌تمام.
- [ ] N · P2 — **Graceful shutdown و drain**؛ فرصت ثبت state و ادامهٔ امن پس از restart.
- [ ] N · P2 — **Circuit breaker و dead-letter handling**؛ الگوهای عمومی ENG برای خطای مکرر یا کار غیرقابل‌پردازش.
- [ ] E · P2 — **بودجهٔ زمان، توکن و تعداد گام‌ها**؛ توقف قابل‌توضیح و خروجی مناسب هنگام exhaustion.

منابع: [Fault tolerance](https://docs.langchain.com/oss/python/langgraph/fault-tolerance)، [Built-in middleware](https://docs.langchain.com/oss/python/langchain/middleware/built-in). Circuit breaker و dead-letter در این فهرست پیشنهاد طراحی زیرساخت‌اند، نه تضمین یک قابلیت آماده در LangGraph.

## 15. الگوهای Agent و Multi-agent

لایه: LC / LG / ENG. هدف: پیچیدگی معماری متناسب با مسئله باشد.

- [ ] O · P2 — **Multi-agent systems**؛ تعریف نقش، ورودی و خروجی هر agent.
- [ ] O · P2 — **Delegation و supervisor**؛ واگذاری کار و جمع‌آوری نتیجه تحت کنترل مرکزی.
- [ ] N · P2 — **Subagent-as-tool در برابر handoff**؛ بازگشت نتیجه به orchestrator یا انتقال کنترل گفتگو.
- [ ] N · P2 — **Router pattern**؛ انتخاب یک یا چند متخصص و ترکیب پاسخ.
- [ ] N · P2 — **Orchestrator-workers**؛ شکستن پویا به کارهای مستقل و aggregation.
- [ ] N · P2 — **Evaluator-optimizer و reflection loops**؛ rubric، بودجه و stop condition برای اصلاح تکراری.
- [ ] N · P1 — **Prompt chaining و validation gates**؛ قرار دادن کنترل قطعی میان مراحل agentic.
- [ ] N · P2 — **Context isolation**؛ هر agent فقط دادهٔ موردنیاز خود را ببیند.
- [ ] N · P2 — **Stateful در برابر stateless specialists**؛ انتخاب scope حافظه و session هر متخصص.
- [ ] N · P2 — **Delegation budget**؛ سقف عمق، تعداد فرزند، هم‌زمانی، زمان و هزینه.
- [ ] N · P2 — **Result contract و conflict resolution**؛ خروجی ساختاریافته و سیاست ادغام پاسخ‌های متناقض.
- [ ] N · P2 — **Single agent + dynamic tools/skills**؛ سنجش اینکه چند agent واقعاً کیفیت یا throughput را بهتر می‌کند.

منابع: [Multi-agent](https://docs.langchain.com/oss/python/langchain/multi-agent)، [Workflows and agents](https://docs.langchain.com/oss/python/langgraph/workflows-agents)، [Subgraphs](https://docs.langchain.com/oss/python/langgraph/use-subgraphs).

## 16. RAG و چرخهٔ عمر داده

لایه: LC / INT / ENG. هدف: retrieval بر دادهٔ سالم و به‌روز انجام شود.

- [ ] O · P1 — **معماری RAG**؛ ingestion، indexing، retrieval، context assembly و generation.
- [ ] N · P1 — **2-step، agentic و hybrid RAG**؛ انتخاب زمان retrieval و کنترل کیفیت میان مراحل.
- [ ] N · P1 — **`Document`**؛ `page_content`، metadata، شناسه و منشأ سند.
- [ ] N · P1 — **Document loaders و parsers**؛ فرق دریافت فایل با استخراج درست متن و ساختار.
- [ ] N · P2 — **`load` در برابر `lazy_load`**؛ مصرف حافظه و پردازش تدریجی corpus بزرگ.
- [ ] N · P2 — **PDF، OCR، جدول و تصویر**؛ ارزیابی کیفیت extraction قبل از تغییر retriever.
- [ ] N · P1 — **Provenance**؛ source URL، صفحه، section و document/chunk ID برای citation قابل‌بررسی.
- [ ] N · P2 — **Deduplication و stable IDs**؛ جلوگیری از duplicate chunk و citationهای مبهم.
- [ ] N · P2 — **Incremental indexing و `RecordManager`**؛ تشخیص سند جدید، تغییرکرده، حذف‌شده یا بدون تغییر.
- [ ] N · P2 — **Upsert، delete و reindex**؛ هماهنگی document store، vector index و نسخهٔ embedding.
- [ ] N · P2 — **Freshness و ingestion failures**؛ ثبت نسخهٔ corpus و بازیابی batchهای ناقص.
- [ ] N · P2 — **Permission-aware ingestion/retrieval**؛ ACL و tenant metadata از منبع trusted وارد شوند.

منابع: [Retrieval](https://docs.langchain.com/oss/python/deepagents/retrieval)، [Document loaders](https://docs.langchain.com/oss/python/integrations/document_loaders/index)، [RecordManager](https://reference.langchain.com/python/langchain-core/indexing/base/RecordManager)، [Indexing API](https://reference.langchain.com/python/langchain-core/indexing).

## 17. Chunking، Embeddings و Vector Stores

لایه: LC / INT / ENG. هدف: واحد جست‌وجو و representation با داده و سؤال سازگار باشد.

- [ ] O · P1 — **Embedding و chunking strategy**؛ انتخاب با evaluation روی corpus واقعی.
- [ ] E · P1 — **Token-based در برابر character-based splitting**؛ اندازهٔ chunk با محدودیت مدل یکسان فرض نشود.
- [ ] N · P1 — **`RecursiveCharacterTextSplitter` و overlap**؛ حفظ مرزهای متن و سنجش تکرار اطلاعات.
- [ ] N · P2 — **Structure-aware splitting**؛ Markdown، HTML، JSON، کد و عنوان بخش‌ها.
- [ ] O · P2 — **Parent/child chunking**؛ جست‌وجوی جزء کوچک و بازگرداندن متن بزرگ‌تر مرتبط.
- [ ] N · P2 — **Multi-vector retrieval**؛ چند embedding برای یک سند و بازگرداندن parent از docstore.
- [ ] N · P1 — **`embed_documents` در برابر `embed_query`**؛ قرارداد embedding سند و پرسش.
- [ ] N · P2 — **ابعاد و نسخهٔ embedding**؛ جلوگیری از مخلوط کردن فضاهای برداری ناسازگار.
- [ ] N · P2 — **Similarity metrics**؛ cosine، dot product و Euclidean و معنای score در backend.
- [ ] N · P2 — **Exact در برابر approximate search**؛ recall، latency و تنظیم index مانند HNSW.
- [ ] N · P1 — **Vector store در برابر retriever**؛ ذخیره و جست‌وجو در برابر قرارداد بازگرداندن سند.
- [ ] N · P2 — **Backend capability matrix**؛ filtering، deletion، async، persistence و multi-tenancy.

منابع: [Text splitters](https://docs.langchain.com/oss/python/integrations/splitters/index)، [Embeddings](https://docs.langchain.com/oss/python/integrations/embeddings/index)، [Vector stores](https://docs.langchain.com/oss/python/integrations/vectorstores/index)، [ParentDocumentRetriever](https://reference.langchain.com/python/langchain-classic/retrievers/parent_document_retriever/ParentDocumentRetriever).

## 18. Retrieval پیشرفته و Context Assembly

لایه: LC / INT / ENG. هدف: سند مرتبط با کمترین اطلاعات مزاحم به مدل برسد.

- [ ] O · P2 — **Hybrid search: BM25 + dense**؛ ترکیب lexical و semantic retrieval.
- [ ] N · P2 — **Reciprocal Rank Fusion و weighting**؛ ادغام رتبه‌ها به‌جای جمع بی‌قاعدهٔ scoreهای متفاوت.
- [ ] N · P1 — **Top-k و score threshold**؛ تنظیم حجم بازیابی و رفتار بدون نتیجه.
- [ ] N · P2 — **MMR**؛ تنوع نتایج و کاهش redundancy در کنار relevance.
- [ ] O · P2 — **Metadata filtering**؛ محدودکردن دامنه با source، زمان، محصول یا tenant.
- [ ] O · P2 — **Reranking**؛ انتخاب مجدد بهترین candidateها با هزینه و latency مشخص.
- [ ] O · P2 — **Query rewriting**؛ تبدیل پرسش مبهم یا follow-up به query مستقل.
- [ ] O · P2 — **Multi-query retrieval**؛ پوشش بیان‌های مختلف و حذف نتیجهٔ تکراری.
- [ ] N · P3 — **Self-query و query decomposition**؛ ساخت filter یا زیربخش‌های پرسش با validation.
- [ ] O · P2 — **Contextual compression**؛ حذف بخش نامرتبط بدون تحریف منبع.
- [ ] N · P2 — **Context packing**؛ deduplication، ordering، token budget و نگهداری citationها.
- [ ] N · P2 — **No-answer و evidence sufficiency**؛ درخواست توضیح یا اعلام نبود شواهد کافی.
- [ ] N · P3 — **Multi-hop، Graph RAG و multimodal retrieval**؛ مسیرهای تخصصی با benchmark مستقل.

منابع: [Retriever integrations](https://docs.langchain.com/oss/python/integrations/retrievers/index)، [EnsembleRetriever](https://reference.langchain.com/python/langchain-classic/retrievers/ensemble/EnsembleRetriever)، [Cohere reranker](https://docs.langchain.com/oss/python/integrations/retrievers/cohere-reranker)، [MongoDB hybrid retrieval](https://docs.langchain.com/oss/python/integrations/retrievers/mongodb_atlas)، [Retrieval](https://docs.langchain.com/oss/python/deepagents/retrieval). نام الگوها مستقل از package است؛ برخی کلاس‌های قدیمی retriever در `langchain-classic` قرار دارند.

## 19. Evaluation و عیب‌یابی RAG

لایه: LS / ENG. هدف: منشأ شکست با اندازه‌گیری جداگانه مشخص شود.

- [ ] O · P1 — **Retrieval failure در برابر generation failure**؛ سند لازم نرسیده یا پاسخ از سند درست ساخته نشده است.
- [ ] E · P1 — **Ingestion و context-assembly failure**؛ مراحل قبل و بعد retrieval هم می‌توانند علت باشند.
- [ ] O · P1 — **Recall@k**؛ سهم اسناد مرتبط بازیابی‌شده از کل اسناد مرتبط در برچسب مرجع.
- [ ] N · P2 — **Precision@k، MRR و nDCG**؛ دقت مجموعه، رتبهٔ اولین پاسخ مرتبط و کیفیت ترتیب نتایج.
- [ ] N · P1 — **Document-level در برابر chunk-level labels**؛ تعریف واحد ارزیابی و جلوگیری از duplicate inflation.
- [ ] N · P1 — **Answer correctness، relevance و groundedness**؛ سه معیار متفاوت برای پاسخ نهایی.
- [ ] N · P2 — **Citation correctness و completeness**؛ آیا منبع ادعا را پشتیبانی می‌کند و ادعاهای لازم منبع دارند؟
- [ ] N · P2 — **No-answer، adversarial و multilingual cases**؛ از جمله متن فارسی و سؤال ترکیبی فارسی/انگلیسی.
- [ ] N · P2 — **Stage-wise ablation**؛ تغییر یک جزء مثل chunking، embedding یا reranker و مقایسه با baseline.
- [ ] N · P2 — **Quality–latency–cost trade-off**؛ ارزیابی بر مجموعهٔ ثابت با نسخهٔ corpus مشخص.

منابع: [Evaluate a RAG application](https://docs.langchain.com/langsmith/evaluate-rag-tutorial)، [Evaluation concepts](https://docs.langchain.com/langsmith/evaluation-concepts)، [Evaluation of ranked retrieval results](https://nlp.stanford.edu/IR-book/html/htmledition/evaluation-of-ranked-retrieval-results-1.html). معیارهای کلاسیک IR و طراحی مجموعهٔ آزمون در این بخش تکمیل مهندسی‌اند؛ وجود آن‌ها به معنی evaluator آماده برای همهٔ معیارها در LangSmith نیست.

## 20. Streaming و تجربهٔ کاربر

لایه: LC / LG؛ رفتار شبکه و reconnect در LS / ENG.

- [ ] O · P1 — **Streaming tokenها و state updates**؛ فرق متن مدل، وضعیت گراف و پیام پیشرفت.
- [ ] E · P1 — **`messages`، `updates`، `values` و `custom`**؛ انتخاب payload مناسب برای مصرف‌کننده.
- [ ] N · P2 — **`AIMessageChunk` و tool-call chunks**؛ جمع‌کردن آرگومان ناقص پیش از مصرف.
- [ ] N · P1 — **Tool-call announcement در برابر tool execution**؛ ظاهر شدن نام ابزار به معنی تکمیل اجرای آن نیست.
- [ ] N · P2 — **Filtering با node، tag و namespace**؛ کنترل streamهای داخلی و subgraphها.
- [ ] N · P2 — **Custom progress events**؛ ارسال پیشرفت واقعی ابزار و node، مستقل از token مدل.
- [ ] N · P2 — **Streaming blockers در LCEL**؛ مرحلهٔ غیرstreaming می‌تواند خروجی را buffer کند.
- [ ] N · P2 — **Guardrail قبل از ارسال chunk**؛ کنترل نهایی پاسخ، دادهٔ ارسال‌شده را پس نمی‌گیرد.
- [ ] N · P2 — **SSE، reconnect، event ID و cancellation**؛ قرارداد stream و عمر اجرای server.
- [ ] N · P2 — **Run stream در برابر thread stream**؛ buffer و قابلیت resume برای هر endpoint بررسی شود.
- [ ] N · P3 — **نسخه‌های event streaming**؛ v2 در `stream` با v3 در `stream_events` و نسخهٔ package یکی نیست.

منابع: [LangGraph streaming](https://docs.langchain.com/oss/python/langgraph/streaming)، [Event streaming](https://docs.langchain.com/oss/python/langchain/event-streaming)، [RunnableSequence](https://reference.langchain.com/python/langchain-core/runnables/base/RunnableSequence)، [Server streaming](https://docs.langchain.com/langsmith/streaming).

## 21. Async، Concurrency و ظرفیت

لایه: LC / LG / ENG. هدف: parallelism به overload یا state corruption تبدیل نشود.

- [ ] O · P2 — **Async و concurrency در production**؛ event loop، I/O و کنترل تعداد کار فعال.
- [ ] N · P2 — **Native async در برابر threadpool wrapper**؛ نام async به‌تنهایی non-blocking بودن همهٔ مسیر را تضمین نمی‌کند.
- [ ] N · P2 — **Blocking work در async node**؛ جداکردن I/O همگام یا محاسبهٔ سنگین از event loop.
- [ ] N · P2 — **Semaphore و bounded concurrency**؛ سقف مشترک برای model، tool و retrieval.
- [ ] N · P2 — **`batch()` در برابر provider Batch API**؛ فراخوانی هم‌زمان client با job دسته‌ای provider فرق دارد.
- [ ] N · P2 — **Cancellation propagation**؛ تعیین تکلیف child taskها و connectionها پس از توقف والد.
- [ ] N · P2 — **Connection pooling و shared-client safety**؛ lifecycle روشن و کنترل محدودیت اتصال DB/API.
- [ ] N · P2 — **Rate limits سراسری و per-tenant**؛ سقف یک process با سقف همهٔ replicaها متفاوت است.
- [ ] N · P2 — **Backpressure و admission control**؛ محدود کردن صف، بار ورودی و حافظهٔ مصرفی.
- [ ] N · P2 — **Load tests و tail latency**؛ throughput، p95/p99، queue time و هزینه زیر بار.

منابع: [Models](https://docs.langchain.com/oss/python/langchain/models)، [Use the Graph API](https://docs.langchain.com/oss/python/langgraph/use-graph-api)، [Scale Agent Server](https://docs.langchain.com/langsmith/agent-server-scale). Semaphore، quota و load-test design تصمیم‌های ENG هستند.

## 22. Cache، Model Routing و هزینه

لایه: LC / LG / INT / ENG. هدف: کاهش هزینه با حفظ درستی و جداسازی داده.

- [ ] O · P2 — **LLM response cache**؛ ذخیرهٔ پاسخ برای ورودی و تنظیمات معادل.
- [ ] O · P2 — **Embedding cache**؛ تفکیک cache سند و query و namespace مدل.
- [ ] O · P3 — **Semantic cache**؛ threshold شباهت و احتمال برگرداندن پاسخ نامناسب.
- [ ] N · P2 — **Node cache و `CachePolicy`**؛ cache نتیجهٔ node جدا از checkpoint و model cache.
- [ ] N · P2 — **Provider prompt caching**؛ کاهش هزینهٔ prefix با response caching فرق دارد.
- [ ] N · P2 — **Cache key design**؛ نسخهٔ prompt/model/schema، corpus و principal مجاز در کلید لحاظ شود.
- [ ] N · P2 — **TTL، invalidation و stale result**؛ تغییر منبع یا مجوز باید رفتار cache را اصلاح کند.
- [ ] N · P1 — **Cache side effects**؛ cache پاسخ ابزار، جای idempotency عملیات خارجی را نمی‌گیرد.
- [ ] O · P2 — **Model fallback و model routing**؛ قابلیت، کیفیت، latency و هزینهٔ مسیر جایگزین.
- [ ] O · P2 — **Cost optimization**؛ بودجهٔ token، ابزار، retrieval، زیرعامل‌ها و eval.
- [ ] N · P2 — **Cost per successful task**؛ هزینهٔ شکست و retry نیز در معیار اقتصادی حساب شود.

منابع: [Redis caching](https://docs.langchain.com/oss/python/integrations/caches/redis_llm_caching)، [CacheBackedEmbeddings](https://reference.langchain.com/python/langchain-classic/embeddings/cache/CacheBackedEmbeddings)، [Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api)، [Models](https://docs.langchain.com/oss/python/langchain/models)، [Built-in middleware](https://docs.langchain.com/oss/python/langchain/middleware/built-in).

## 23. Observability، Tracing و Prompt Operations

لایه: LS / ENG. هدف: رفتار واقعی سیستم به نسخهٔ دقیق اجزای آن متصل باشد.

- [ ] O · P1 — **Observability و tracing با LangSmith**؛ مشاهدهٔ فراخوانی مدل، ابزار، retrieval و خطا.
- [ ] N · P1 — **Run، trace، thread و trajectory**؛ واحد کار، درخت اجرا، جلسه و توالی پیام‌ها.
- [ ] N · P1 — **Tracing thread در برابر checkpoint thread**؛ metadata ردیابی به‌تنهایی persistence ایجاد نمی‌کند.
- [ ] N · P2 — **Automatic tracing و `@traceable`**؛ پوشش کد سفارشی و اجزای غیرLangChain.
- [ ] N · P2 — **Distributed trace propagation**؛ حفظ رابطهٔ parent/child در سرویس‌ها و workerها.
- [ ] N · P2 — **Tags و release metadata**؛ ثبت نسخهٔ کد، prompt، مدل، schema و corpus.
- [ ] N · P2 — **Latency و cost breakdown**؛ زمان مدل، ابزار، retrieval، اولین token و کل task.
- [ ] N · P2 — **Sampling و conditional tracing**؛ انتخاب دادهٔ observability و کنترل قطعی دادهٔ حساس.
- [ ] N · P2 — **Trace redaction و retention**؛ حذف PII و secrets پیش از export و تعیین عمر داده.
- [ ] N · P2 — **User feedback و online scores**؛ پیوند بازخورد به اجرای قابل‌بررسی.
- [ ] E · P2 — **Prompt commits، tags و promotion**؛ نسخهٔ pinشده، staging، production و rollback.

منابع: [Observability concepts](https://docs.langchain.com/langsmith/observability-concepts)، [Sensitive trace data](https://docs.langchain.com/langsmith/mask-inputs-outputs)، [Sampling](https://docs.langchain.com/langsmith/sample-traces)، [Conditional tracing](https://docs.langchain.com/langsmith/conditional-tracing)، [Manage prompts](https://docs.langchain.com/langsmith/manage-prompts).

## 24. Testing و Evaluation برای Agent

لایه: LC / LG / LS / ENG. هدف: درستی برنامه از کیفیت احتمالی مدل تفکیک شود.

- [ ] O · P1 — **Unit test برای nodeها و LangGraph**؛ فراخوانی مستقیم `graph.nodes` checkpointer را دور می‌زند؛ persistence با graph کامپایل‌شده آزموده شود.
- [ ] N · P1 — **آزمون reducer، routing، tool و middleware**؛ آزمودن منطق قطعی مستقل از مدل واقعی.
- [ ] N · P1 — **Fake/mocked در برابر live integration test**؛ نتیجهٔ mock تأیید رفتار provider واقعی نیست.
- [ ] N · P2 — **Fresh checkpointer و isolated thread fixtures**؛ جلوگیری از اثر state آزمون قبلی.
- [ ] N · P2 — **Partial graph و subgraph tests**؛ آزمون مسیر محدود بدون اجرای تمام workflow.
- [ ] N · P2 — **Recovery، retry و interrupt tests**؛ crash، approve/edit/reject و جلوگیری از side effect تکراری.
- [ ] O · P1 — **Agent evaluation و trajectory**؛ سنجش تصمیم‌ها و tool calls علاوه بر پاسخ نهایی.
- [ ] E · P2 — **Trajectory matching**؛ strict، unordered، subset و superset و صحت آرگومان ابزار.
- [ ] O · P1 — **Datasets، experiments و evaluators در LangSmith**؛ دادهٔ آزمون، اجرای مقایسه و معیار.
- [ ] N · P2 — **Dataset provenance، version و splits**؛ جداسازی توسعه و ارزیابی و ثبت منشأ نمونه.
- [ ] N · P2 — **Deterministic، LLM-as-judge، human و pairwise evaluation**؛ انتخاب متناسب با معیار.
- [ ] N · P2 — **Reference-based در برابر reference-free**؛ تفاوت قضاوت با پاسخ مرجع و بدون آن.
- [ ] N · P2 — **Judge calibration و annotation queues**؛ rubric بازبینی، برچسب انسانی و سنجش توافق داور.
- [ ] N · P2 — **Repetitions و variance**؛ چند اجرای یک نمونه برای سنجش ناپایداری agent.
- [ ] O · P2 — **Integration و regression evaluation**؛ baseline، آستانهٔ افت کیفیت و CI gate.
- [ ] N · P2 — **Offline در برابر online evaluation**؛ تبدیل failureهای production به نمونهٔ regression.
- [ ] N · P2 — **Multi-turn evaluation**؛ ارجاع به پیام قبل، حفظ اطلاعات و موفقیت کل گفتگو.
- [ ] N · P2 — **Evaluation budgets**؛ هزینه، concurrency، latency و اندازهٔ نمونهٔ مناسب.

منابع: [LangChain testing](https://docs.langchain.com/oss/python/langchain/test)، [LangGraph testing](https://docs.langchain.com/oss/python/langgraph/test)، [Trajectory evaluations](https://docs.langchain.com/langsmith/trajectory-evals)، [Evaluation concepts](https://docs.langchain.com/langsmith/evaluation-concepts)، [Evaluation approaches](https://docs.langchain.com/langsmith/evaluation-approaches)، [Repetitions](https://docs.langchain.com/langsmith/repetition)، [Evaluate an application](https://docs.langchain.com/langsmith/evaluate-llm-application).

## 25. امنیت، مجوز و حفاظت از داده

لایه: LC / LS / ENG. هدف: اختیار و جریان داده در مرزهای trusted کنترل شوند.

- [ ] O · P1 — **Prompt injection و indirect prompt injection**؛ دستور مخرب در پیام، سند، ابزار یا memory.
- [ ] N · P1 — **Trust boundaries**؛ متن بازیابی‌شده، خروجی ابزار و پیام subagent دادهٔ trusted محسوب نشوند.
- [ ] O · P1 — **Tool authorization و least privilege**؛ بررسی مجوز در نقطهٔ اجرا با identity معتبر.
- [ ] N · P1 — **Authentication در برابر authorization**؛ شناخت کاربر با اجازهٔ دسترسی به resource فرق دارد.
- [ ] N · P1 — **Tenant isolation**؛ thread، store، vector retrieval، cache و trace متعلق به کاربر درست باشند.
- [ ] O · P1 — **Data exfiltration**؛ محدودیت مقصد شبکه، محتوای خروجی و دادهٔ قابل‌دسترسی ابزار.
- [ ] N · P2 — **Secret management و delegated credentials**؛ secrets بیرون prompt، state و sandbox و پشت ابزار محدود یا proxy معتبر بمانند.
- [ ] N · P2 — **Sandboxing**؛ کنترل فایل، اجرا و شبکه؛ sandbox جای کنترل prompt injection را نمی‌گیرد.
- [ ] N · P1 — **Business invariants و parameter validation**؛ اجازهٔ عملیات به ادعای مدل وابسته نباشد.
- [ ] N · P2 — **PII و streaming guardrails**؛ ورودی، خروجی ابزار، chunkها و traceها جدا بررسی شوند.
- [ ] N · P2 — **Unsafe deserialization و template rendering**؛ checkpoint یا template ناشناس trusted فرض نشود.
- [ ] N · P2 — **Memory poisoning و unsafe output consumption**؛ دادهٔ تولیدشده مستقیم به SQL، shell یا HTML اجرایی تبدیل نشود.
- [ ] N · P2 — **Security regression suite**؛ آزمون injection، عبور از مجوز، نشت داده و دسترسی cross-tenant.

منابع: [Security policy](https://docs.langchain.com/oss/python/security-policy)، [Guardrails](https://docs.langchain.com/oss/python/langchain/guardrails)، [Authentication and access control](https://docs.langchain.com/langsmith/auth)، [Sandboxes](https://docs.langchain.com/oss/python/deepagents/sandboxes)، [Sensitive trace data](https://docs.langchain.com/langsmith/mask-inputs-outputs).

## 26. Deployment، Agent Server و APIها

لایه: LS Deployment؛ استفاده از OSS در server شخصی مسیر جداگانهٔ ENG است.

- [ ] O · P2 — **Deployment**؛ انتخاب app شخصی با LangGraph یا Agent Server با سرویس‌های آماده.
- [ ] N · P2 — **Graph، assistant، thread و run**؛ کد، پیکربندی، state گفتگو و invocation.
- [ ] N · P2 — **`langgraph.json` و application structure**؛ graph export، dependencies و environment.
- [ ] N · P2 — **LangGraph CLI**؛ کاربرد `dev`، `build` و `up` و تفاوت اجرای توسعه با production.
- [ ] N · P2 — **LangSmith Studio**؛ مشاهدهٔ graph، state، thread و بررسی execution.
- [ ] N · P2 — **REST API، Python/JS SDK و `RemoteGraph`**؛ انتخاب روش اتصال client یا سرویس دیگر.
- [ ] N · P2 — **Stateful و stateless runs**؛ invocation با thread پایدار یا بدون آن.
- [ ] N · P2 — **Background runs، wait، join و cancel**؛ lifecycle کار پس از خروج درخواست HTTP.
- [ ] N · P2 — **Client disconnect policy**؛ قطع اتصال کاربر لزوماً همان cancel شدن اجرا نیست.
- [ ] N · P2 — **Double texting**؛ enqueue، reject، interrupt و rollback برای درخواست‌های هم‌زمان یک thread.
- [ ] N · P3 — **Cron و webhooks**؛ اجرای زمان‌بندی‌شده و اطلاع‌رسانی پایان کار.
- [ ] N · P2 — **Managed persistence در Agent Server**؛ مسئولیت checkpointer/store را با حالت OSS دستی اشتباه نگیرید.

منابع: [Agent Server](https://docs.langchain.com/langsmith/agent-server)، [Application structure](https://docs.langchain.com/langsmith/application-structure)، [CLI](https://docs.langchain.com/langsmith/cli)، [Studio](https://docs.langchain.com/langsmith/studio)، [Runs](https://docs.langchain.com/langsmith/runs)، [Double texting](https://docs.langchain.com/langsmith/double-texting)، [Webhooks](https://docs.langchain.com/langsmith/use-webhooks).

## 27. زیرساخت Production و عملیات

لایه: LS / ENG. هدف: مقیاس، مالکیت داده و بازیابی عملیاتی مشخص باشد.

- [ ] O · P2 — **Horizontal scaling و queue/worker architecture**؛ جداسازی پذیرش درخواست از اجرای کار.
- [ ] N · P2 — **API replicas در برابر worker replicas**؛ کنترل مستقل ظرفیت دریافت و ظرفیت اجرا.
- [ ] N · P2 — **Durable run storage در برابر signaling**؛ در Agent Server، PostgreSQL و Redis نقش یکسان ندارند.
- [ ] N · P2 — **Per-thread concurrency policy**؛ از update هم‌زمان و بدون کنترل یک state جلوگیری شود.
- [ ] N · P2 — **Backlog، retry storm و saturation**؛ سنجه و سیاست کاهش فشار برای صف و backend.
- [ ] N · P2 — **Health، readiness و graceful shutdown**؛ رفتار replica در شروع، اختلال و پایان.
- [ ] N · P2 — **Database backup و restore**؛ checkpointer، store و دادهٔ resourceها جدا شناسایی شوند.
- [ ] N · P2 — **TTL و data deletion**؛ lifecycle گفتگو، checkpoint، memory، trace و artifact.
- [ ] N · P2 — **Cloud، hybrid و self-hosted**؛ auth محیط self-hosted باید صریحاً تنظیم شود؛ پیش‌فرض cloud را فرض نکنید.
- [ ] N · P2 — **CI/CD، canary و rollback**؛ ارزیابی نسخهٔ جدید پیش از انتقال کامل بار.
- [ ] N · P2 — **SLO، alerts و runbooks**؛ تعریف موفقیت، زمان پاسخ و اقدام هنگام شکست.
- [ ] N · P2 — **Capacity و cost planning**؛ مصرف CPU/RAM، اتصال DB، توکن و محدودیت provider.

منابع: [Scale Agent Server](https://docs.langchain.com/langsmith/agent-server-scale)، [Agent Server](https://docs.langchain.com/langsmith/agent-server)، [Authentication](https://docs.langchain.com/langsmith/auth)، [Fault tolerance](https://docs.langchain.com/oss/python/langgraph/fault-tolerance). Canary، SLO و runbook توصیه‌های ENG هستند.

## 28. State Migration و سازگاری اجراهای فعال

لایه: LG / ENG. هدف: deploy جدید گفتگوهای قدیمی یا approvalهای معلق را خراب نکند.

- [ ] O · P2 — **State migrations و versioning**؛ قرارداد دادهٔ ذخیره‌شده و کد مصرف‌کننده.
- [ ] N · P2 — **Schema compatibility در برابر behavioral compatibility**؛ خوانده‌شدن state، حفظ رفتار را تضمین نمی‌کند.
- [ ] N · P2 — **Optional/defaulted fields**؛ روش‌های افزودن فیلد بدون شکستن checkpoint قدیمی.
- [ ] N · P2 — **Node rename/delete**؛ اثر تغییر topology بر thread متوقف‌شده روی node قبلی.
- [ ] N · P2 — **تغییر type، key و reducer**؛ migration صریح و آزمون stateهای قدیمی.
- [ ] N · P2 — **Behavior/flow version**؛ نگه‌داشتن مسیر مناسب برای threadهای ایجادشده با منطق قبلی.
- [ ] N · P2 — **Functional task/interrupt ordering**؛ reorder می‌تواند بازخوانی نتایج ذخیره‌شده را ناسازگار کند.
- [ ] N · P2 — **Migration rehearsal**؛ آزمون checkpointهای نمونه در staging و inventory اجراهای فعال.
- [ ] N · P2 — **Drain یا مسیر جدا برای تغییر شکستن‌ساز**؛ انتخاب آگاهانهٔ سرنوشت in-flight workflows.

منابع: [Backward compatibility](https://docs.langchain.com/oss/python/langgraph/backward-compatibility)، [Functional API](https://docs.langchain.com/oss/python/langgraph/functional-api)، [Fault tolerance](https://docs.langchain.com/oss/python/langgraph/fault-tolerance).

## 29. MCP و Interoperability

لایه: LC / INT؛ برخی endpointها متعلق به LS هستند.

- [ ] O · P2 — **MCP و LangChain**؛ مدل، MCP client، adapter و MCP server نقش‌های جدا دارند.
- [ ] N · P2 — **Tools، resources و prompts**؛ قابلیت پروتکل با پوشش adapter یکی نیست.
- [ ] N · P2 — **stdio و Streamable HTTP**؛ lifecycle اتصال محلی یا سرویس راه‌دور.
- [ ] N · P2 — **Session lifetime، pooling و discovery cache**؛ طول عمر اتصال با طول عمر agent یکسان نیست.
- [ ] N · P2 — **چند server و tool-name collision**؛ namespace و منشأ هر ابزار روشن باشد.
- [ ] N · P2 — **Server/user authentication**؛ credential و tool visibility متناسب با principal.
- [ ] N · P2 — **MCP result و error semantics**؛ structured content، artifact، `isError` و transport exception.
- [ ] N · P3 — **Elicitation و interrupt/resume**؛ دریافت داده از کاربر در میانهٔ درخواست ابزار.
- [ ] N · P2 — **Adapter migration**؛ تفکیک APIهای `langchain-mcp-adapters` از `langchain.mcp` جدید و آزمایشی.
- [ ] N · P3 — **Consume tools در برابر expose agent**؛ اتصال به server ابزار با انتشار Agent Server به‌صورت MCP فرق دارد.
- [ ] N · P3 — **A2A، ACP و AG-UI**؛ شناخت سطحی تا زمانی که integration واقعی به آن‌ها نیاز داشته باشد.

منابع: [MCP](https://docs.langchain.com/oss/python/langchain/mcp)، [MCP connections](https://docs.langchain.com/oss/python/langchain/mcp/connections)، [MCP migration](https://docs.langchain.com/oss/python/migrate/langchain-mcp-adapters)، [Agent Server API](https://docs.langchain.com/langsmith/server-api-ref).

## 30. مباحث جدید، تخصصی و اختیاری

لایه: LG / LC / INT. این بخش پیش‌نیاز یادگیری هسته نیست؛ APIهای آزمایشی باید با نسخهٔ نصب‌شده تطبیق داده شوند.

- [ ] N · P3 — **Typed event streaming v3**؛ projectionهای messages، output، tool calls و subagents.
- [ ] N · P3 — **`TimeoutPolicy`، idle timeout و heartbeat**؛ قابلیت‌های جدید fault tolerance و محدودیت پشتیبانی async.
- [ ] N · P3 — **`NodeError`، `error_handler` و `set_node_defaults`**؛ سیاست مشترک خطا و override هر node.
- [ ] N · P3 — **`RunControl` و cooperative drain**؛ توقف در مرز مناسب اجرا؛ drain کار درحال‌اجرا را فوراً cancel نمی‌کند.
- [ ] N · P3 — **`runtime.execution_info` و server identity**؛ metadata اجرای node با context ورودی فرق دارد.
- [ ] N · P3 — **Pregel channels و `DeltaChannel` آزمایشی**؛ ذخیرهٔ delta، بازسازی state و سازگاری نسخه‌ها.
- [ ] N · P3 — **Custom checkpointer، serializer و encryption**؛ قرارداد backend و آزمون سازگاری.
- [ ] N · P3 — **Dynamic tool discovery و headless/client tools**؛ مجموعهٔ ابزار بزرگ و اجرای سمت client.
- [ ] N · P3 — **Deep Agents**؛ harness جداگانه برای filesystem context، subagents، skills و planning اختیاری.
- [ ] N · P3 — **OpenEvals و AgentEvals**؛ استفاده از evaluatorهای قابل‌بازاستفاده در کنار معیار سفارشی.

منابع: [Event streaming](https://docs.langchain.com/oss/python/langgraph/event-streaming)، [Fault tolerance](https://docs.langchain.com/oss/python/langgraph/fault-tolerance)، [Pregel](https://docs.langchain.com/oss/python/langgraph/pregel)، [Runtime](https://docs.langchain.com/oss/python/langchain/runtime)، [Tools](https://docs.langchain.com/oss/python/langchain/tools)، [Deep Agents overview](https://docs.langchain.com/oss/python/deepagents/overview)، [Trajectory evaluations](https://docs.langchain.com/langsmith/trajectory-evals).

## تمایزهای مهم برای جلوگیری از برداشت نادرست

**۱. State schema با output schema متفاوت است.** در `create_agent`، custom state بر پایهٔ `TypedDict`/`AgentState` است؛ Pydantic همچنان برای خروجی ساختاریافته و ورودی ابزار کاربرد دارد. این محدودیت را به همهٔ schemaهای `StateGraph` تعمیم ندهید. [Migration guide](https://docs.langchain.com/oss/python/migrate/langchain-v1)

**۲. Checkpoint، context و منبع حقیقت سه نقش متفاوت دارند.** checkpoint وضعیت اجرا را ذخیره می‌کند؛ context اطلاعات ارائه‌شده به مدل است؛ DB کسب‌وکار حقیقت معتبر عملیاتی را نگه می‌دارد. دادهٔ critical می‌تواند در state نیز باشد، اما checkpoint به‌تنهایی تراکنش کسب‌وکار یا صحت اطلاعات را تضمین نمی‌کند. [Persistence](https://docs.langchain.com/oss/python/langgraph/persistence)

**۳. Resume و replay اجرای بدون side effect تضمین نمی‌کنند.** node متوقف‌شده هنگام resume دوباره شروع می‌شود؛ replay پس از checkpoint انتخابی می‌تواند فراخوانی‌های خارجی را تکرار کند. اثرهای خارجی به idempotency و در صورت نیاز عملیات جبرانی نیاز دارند. [Interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts)، [Time travel](https://docs.langchain.com/oss/python/langgraph/use-time-travel)

**۴. موضوعات حافظهٔ RAG با حافظهٔ گفتگو یکی نیستند.** vector store می‌تواند دانش اسناد را نگه دارد؛ checkpointer ادامهٔ اجرای یک thread را حفظ می‌کند؛ Store می‌تواند دادهٔ میان threadها را نگه دارد. انتخاب یک backend، نقش منطقی این اجزا را یکسان نمی‌کند. [Stores](https://docs.langchain.com/oss/python/langgraph/stores)، [Vector stores](https://docs.langchain.com/oss/python/integrations/vectorstores/index)

**۵. Private state به معنی redaction نیست.** schema داخلی را کنترل محرمانگی stream یا trace فرض نکنید؛ دادهٔ خروجی باید صریحاً محدود و پاک‌سازی شود. [Streaming](https://docs.langchain.com/oss/python/langgraph/streaming)، [Sensitive trace data](https://docs.langchain.com/langsmith/mask-inputs-outputs)

**۶. `batch()`، async و streaming سه مفهوم متفاوت‌اند.** اولی چند ورودی را اجرا می‌کند، دومی سبک انتظار برای عملیات را مشخص می‌کند و سومی خروجی تدریجی می‌دهد. `batch()` لزوماً از Batch API اختصاصی provider استفاده نمی‌کند. [Models](https://docs.langchain.com/oss/python/langchain/models)

**۷. وجود قالب JSON به معنی پاسخ درست نیست.** validation ساختار را بررسی می‌کند؛ صحت ادعا، مالکیت داده و مجاز بودن عملیات نیازمند کنترل جداگانه است. این یک اصل طراحی ENG است. [Structured output](https://docs.langchain.com/oss/python/langchain/structured-output)

**۸. قابلیت Deployment را به OSS نسبت ندهید.** Double texting یک قابلیت LangSmith Deployment است. در Agent Server، PostgreSQL دادهٔ پایدار اجرا را نگه می‌دارد و Redis برای signaling/pubsub موقت به‌کار می‌رود. [Double texting](https://docs.langchain.com/langsmith/double-texting)، [Agent Server](https://docs.langchain.com/langsmith/agent-server)

**۹. Hybrid RAG با hybrid search یکی نیست.** Hybrid RAG ترکیب کنترل‌های workflow و رفتار agentic در معماری پاسخ‌گویی است؛ hybrid search ترکیب روش‌های جست‌وجو، مثلاً sparse و dense، در مرحلهٔ retrieval است. [Retrieval](https://docs.langchain.com/oss/python/deepagents/retrieval)، [MongoDB hybrid retrieval](https://docs.langchain.com/oss/python/integrations/retrievers/mongodb_atlas)

## ترتیب پیشنهادی مطالعه

- **مرحلهٔ ۱ — قراردادهای پایه:** بخش‌های 01 تا 05؛ خروجی عملی: مدل، پیام، ابزار و پاسخ ساختاریافته با validation.
- **مرحلهٔ ۲ — Agent و حافظه:** بخش‌های 06 تا 08؛ خروجی عملی: دو thread مستقل، memory مشخص و context محدود.
- **مرحلهٔ ۳ — کنترل جریان:** بخش‌های 09 تا 11 و 15؛ خروجی عملی: workflow دارای routing، join و توقف قطعی.
- **مرحلهٔ ۴ — دوام و تعامل انسانی:** بخش‌های 12 تا 14 و 28؛ خروجی عملی: crash/restart و approval بدون side effect تکراری.
- **مرحلهٔ ۵ — RAG قابل‌اندازه‌گیری:** بخش‌های 16 تا 19؛ خروجی عملی: ingestion سالم، citation و معیارهای جداگانهٔ retrieval/generation.
- **مرحلهٔ ۶ — مشاهده و ارزیابی:** بخش‌های 20 تا 25؛ tracing و تست‌ها بهتر است از مراحل قبلی شروع شوند؛ اینجا کامل و مقایسه‌پذیر می‌شوند.
- **مرحلهٔ ۷ — Production و اتصال‌ها:** بخش‌های 26، 27 و 29؛ بخش 30 فقط بر اساس نیاز پروژه. کنترل‌های P1 امنیت باید از اولین ابزار فعال باشند.

## دامنه، وضعیت نسخه‌ها و محدودیت پوشش

این فهرست یک پوشش موضوعی جامع از راهنماهای اصلی و مرجع‌های مرتبط است؛ ادعای خواندن تک‌تک صفحات، همهٔ integrationها یا همهٔ symbolهای API را ندارد. مبنا مستندات رسمی Python و منابع مستقیم LangChain/LangGraph/LangSmith در تاریخ **۱۱ سپتامبر ۲۰۲۶** است. تاریخ انتشار بسیاری از صفحات مشخص نشده و مستندات زنده ممکن است تغییر کنند.

در مستندات فعلی، بخشی از مطالب از مسیرهای قدیمی به مسیرهای جدید، از جمله زیرمجموعهٔ Deep Agents، منتقل شده‌اند؛ این جابه‌جایی به‌تنهایی به معنی وابستگی همهٔ الگوهای RAG به Deep Agents نیست. بسته‌های partner، `langchain-classic` و APIهای core باید جدا بررسی شوند. بعضی صفحات integration اکنون دربارهٔ نگهداری‌نشدن `langchain-community` هشدار دارند؛ این به معنی خراب بودن تمام نمونه‌های موجود نیست. [Integration maintenance note](https://docs.langchain.com/oss/python/integrations/embeddings/instruct_embeddings)

در زمان بررسی، راهنمای MCP، `langchain.mcp.MCPAdapter` را **آزمایشی** و نیازمند `langchain[mcp]>=1.4.0` معرفی می‌کند؛ resources و prompts پوشش wrapper یکسانی با tools ندارند. راهنمای fault tolerance نیز قابلیت‌هایی برای `langgraph>=1.2` دارد و `DeltaChannel` آزمایشی است. این شماره‌ها حداقل‌های ذکرشده در صفحات مربوط‌اند، نه یک مجموعهٔ dependency تأییدشده برای نصب با هم. [MCP](https://docs.langchain.com/oss/python/langchain/mcp)، [MCP migration](https://docs.langchain.com/oss/python/migrate/langchain-mcp-adapters)، [Fault tolerance](https://docs.langchain.com/oss/python/langgraph/fault-tolerance)، [Pregel](https://docs.langchain.com/oss/python/langgraph/pregel)

برخی مثال‌های صفحهٔ Graph API زبان‌های مختلف را کنار هم نمایش می‌دهند. نام‌های مخصوص TypeScript نباید صرفاً از روی صفحهٔ Python، API پایتون فرض شوند. در این گزارش، نام‌های `StateSchema`، `GraphNode` و `UntrackedValue` به‌عنوان API تأییدشدهٔ Python وارد چک‌لیست نشده‌اند.

معیارهای IR، policyهای cache، SLO، isolation، migration rehearsal و ظرفیت‌سنجی شامل تحلیل و توصیهٔ مهندسی نیز هستند؛ مستند بودن یک primitive به معنی پیاده‌سازی خودکار همهٔ این الزامات نیست. شاخه‌های بازاریابی و همهٔ محصولات مجاور عمداً پیش‌نیاز یادگیری هسته معرفی نشده‌اند.

## منابع کامل

تاریخ دسترسی همهٔ منابع: ۱۱ سپتامبر ۲۰۲۶. صفحات مستندات زنده‌اند و تاریخ انتشار ثابت در اغلب آن‌ها درج نشده است.

1. LangChain — مستندات و مرجع رسمی. [LangChain overview](https://docs.langchain.com/oss/python/langchain/overview).
2. LangChain — مستندات و مرجع رسمی. [LangGraph overview](https://docs.langchain.com/oss/python/langgraph/overview).
3. LangChain — مستندات و مرجع رسمی. [LangChain v1 migration](https://docs.langchain.com/oss/python/migrate/langchain-v1).
4. LangChain — مستندات و مرجع رسمی. [Models](https://docs.langchain.com/oss/python/langchain/models).
5. LangChain — مستندات و مرجع رسمی. [Messages](https://docs.langchain.com/oss/python/langchain/messages).
6. LangChain — مستندات و مرجع رسمی. [Tools](https://docs.langchain.com/oss/python/langchain/tools).
7. LangChain — مستندات و مرجع رسمی. [Runnables reference](https://reference.langchain.com/python/langchain-core/runnables).
8. LangChain — مستندات و مرجع رسمی. [Structured output](https://docs.langchain.com/oss/python/langchain/structured-output).
9. LangChain — مستندات و مرجع رسمی. [Chat prompt reference](https://reference.langchain.com/python/langchain-core/prompts/chat).
10. LangChain — مستندات و مرجع رسمی. [Manage prompts](https://docs.langchain.com/langsmith/manage-prompts).
11. LangChain — مستندات و مرجع رسمی. [Custom middleware](https://docs.langchain.com/oss/python/langchain/middleware/custom).
12. LangChain — مستندات و مرجع رسمی. [Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api).
13. LangChain — مستندات و مرجع رسمی. [Middleware overview](https://docs.langchain.com/oss/python/langchain/middleware/overview).
14. LangChain — مستندات و مرجع رسمی. [Built-in middleware](https://docs.langchain.com/oss/python/langchain/middleware/built-in).
15. LangChain — مستندات و مرجع رسمی. [Context engineering](https://docs.langchain.com/oss/python/langchain/context-engineering).
16. LangChain — مستندات و مرجع رسمی. [Short-term memory](https://docs.langchain.com/oss/python/langchain/short-term-memory).
17. LangChain — مستندات و مرجع رسمی. [Persistence](https://docs.langchain.com/oss/python/langgraph/persistence).
18. LangChain — مستندات و مرجع رسمی. [Long-term memory](https://docs.langchain.com/oss/python/langchain/long-term-memory).
19. LangChain — مستندات و مرجع رسمی. [Stores](https://docs.langchain.com/oss/python/langgraph/stores).
20. LangChain — مستندات و مرجع رسمی. [Memory concepts](https://docs.langchain.com/oss/python/concepts/memory).
21. LangChain — مستندات و مرجع رسمی. [Use the Graph API](https://docs.langchain.com/oss/python/langgraph/use-graph-api).
22. LangChain — مستندات و مرجع رسمی. [Pregel runtime](https://docs.langchain.com/oss/python/langgraph/pregel).
23. LangChain — مستندات و مرجع رسمی. [Functional API](https://docs.langchain.com/oss/python/langgraph/functional-api).
24. LangChain — مستندات و مرجع رسمی. [Subgraphs](https://docs.langchain.com/oss/python/langgraph/use-subgraphs).
25. LangChain — مستندات و مرجع رسمی. [Checkpointers](https://docs.langchain.com/oss/python/langgraph/checkpointers).
26. LangChain — مستندات و مرجع رسمی. [Interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts).
27. LangChain — مستندات و مرجع رسمی. [Time travel](https://docs.langchain.com/oss/python/langgraph/use-time-travel).
28. LangChain — مستندات و مرجع رسمی. [Fault tolerance](https://docs.langchain.com/oss/python/langgraph/fault-tolerance).
29. LangChain — مستندات و مرجع رسمی. [Multi-agent](https://docs.langchain.com/oss/python/langchain/multi-agent).
30. LangChain — مستندات و مرجع رسمی. [Workflows and agents](https://docs.langchain.com/oss/python/langgraph/workflows-agents).
31. LangChain — مستندات و مرجع رسمی. [Retrieval](https://docs.langchain.com/oss/python/deepagents/retrieval).
32. LangChain — مستندات و مرجع رسمی. [Document loaders](https://docs.langchain.com/oss/python/integrations/document_loaders/index).
33. LangChain — مستندات و مرجع رسمی. [RecordManager](https://reference.langchain.com/python/langchain-core/indexing/base/RecordManager).
34. LangChain — مستندات و مرجع رسمی. [Indexing API](https://reference.langchain.com/python/langchain-core/indexing).
35. LangChain — مستندات و مرجع رسمی. [Text splitters](https://docs.langchain.com/oss/python/integrations/splitters/index).
36. LangChain — مستندات و مرجع رسمی. [Embeddings](https://docs.langchain.com/oss/python/integrations/embeddings/index).
37. LangChain — مستندات و مرجع رسمی. [Vector stores](https://docs.langchain.com/oss/python/integrations/vectorstores/index).
38. LangChain — مستندات و مرجع رسمی. [ParentDocumentRetriever](https://reference.langchain.com/python/langchain-classic/retrievers/parent_document_retriever/ParentDocumentRetriever).
39. LangChain — مستندات و مرجع رسمی. [Retriever integrations](https://docs.langchain.com/oss/python/integrations/retrievers/index).
40. LangChain — مستندات و مرجع رسمی. [EnsembleRetriever](https://reference.langchain.com/python/langchain-classic/retrievers/ensemble/EnsembleRetriever).
41. LangChain — مستندات و مرجع رسمی. [Cohere reranker](https://docs.langchain.com/oss/python/integrations/retrievers/cohere-reranker).
42. LangChain — مستندات و مرجع رسمی. [MongoDB hybrid retrieval](https://docs.langchain.com/oss/python/integrations/retrievers/mongodb_atlas).
43. LangChain — مستندات و مرجع رسمی. [Evaluate a RAG application](https://docs.langchain.com/langsmith/evaluate-rag-tutorial).
44. LangChain — مستندات و مرجع رسمی. [Evaluation concepts](https://docs.langchain.com/langsmith/evaluation-concepts).
45. Manning, Raghavan & Schütze — Introduction to Information Retrieval, Cambridge University Press, 2008. [Evaluation of ranked retrieval results](https://nlp.stanford.edu/IR-book/html/htmledition/evaluation-of-ranked-retrieval-results-1.html).
46. LangChain — مستندات و مرجع رسمی. [LangGraph streaming](https://docs.langchain.com/oss/python/langgraph/streaming).
47. LangChain — مستندات و مرجع رسمی. [Event streaming](https://docs.langchain.com/oss/python/langchain/event-streaming).
48. LangChain — مستندات و مرجع رسمی. [RunnableSequence](https://reference.langchain.com/python/langchain-core/runnables/base/RunnableSequence).
49. LangChain — مستندات و مرجع رسمی. [Server streaming](https://docs.langchain.com/langsmith/streaming).
50. LangChain — مستندات و مرجع رسمی. [Scale Agent Server](https://docs.langchain.com/langsmith/agent-server-scale).
51. LangChain — مستندات و مرجع رسمی. [Redis caching](https://docs.langchain.com/oss/python/integrations/caches/redis_llm_caching).
52. LangChain — مستندات و مرجع رسمی. [CacheBackedEmbeddings](https://reference.langchain.com/python/langchain-classic/embeddings/cache/CacheBackedEmbeddings).
53. LangChain — مستندات و مرجع رسمی. [Observability concepts](https://docs.langchain.com/langsmith/observability-concepts).
54. LangChain — مستندات و مرجع رسمی. [Sensitive trace data](https://docs.langchain.com/langsmith/mask-inputs-outputs).
55. LangChain — مستندات و مرجع رسمی. [Sampling](https://docs.langchain.com/langsmith/sample-traces).
56. LangChain — مستندات و مرجع رسمی. [Conditional tracing](https://docs.langchain.com/langsmith/conditional-tracing).
57. LangChain — مستندات و مرجع رسمی. [LangChain testing](https://docs.langchain.com/oss/python/langchain/test).
58. LangChain — مستندات و مرجع رسمی. [LangGraph testing](https://docs.langchain.com/oss/python/langgraph/test).
59. LangChain — مستندات و مرجع رسمی. [Trajectory evaluations](https://docs.langchain.com/langsmith/trajectory-evals).
60. LangChain — مستندات و مرجع رسمی. [Evaluation approaches](https://docs.langchain.com/langsmith/evaluation-approaches).
61. LangChain — مستندات و مرجع رسمی. [Repetitions](https://docs.langchain.com/langsmith/repetition).
62. LangChain — مستندات و مرجع رسمی. [Evaluate an application](https://docs.langchain.com/langsmith/evaluate-llm-application).
63. LangChain — مستندات و مرجع رسمی. [Security policy](https://docs.langchain.com/oss/python/security-policy).
64. LangChain — مستندات و مرجع رسمی. [Guardrails](https://docs.langchain.com/oss/python/langchain/guardrails).
65. LangChain — مستندات و مرجع رسمی. [Authentication and access control](https://docs.langchain.com/langsmith/auth).
66. LangChain — مستندات و مرجع رسمی. [Sandboxes](https://docs.langchain.com/oss/python/deepagents/sandboxes).
67. LangChain — مستندات و مرجع رسمی. [Agent Server](https://docs.langchain.com/langsmith/agent-server).
68. LangChain — مستندات و مرجع رسمی. [Application structure](https://docs.langchain.com/langsmith/application-structure).
69. LangChain — مستندات و مرجع رسمی. [CLI](https://docs.langchain.com/langsmith/cli).
70. LangChain — مستندات و مرجع رسمی. [Studio](https://docs.langchain.com/langsmith/studio).
71. LangChain — مستندات و مرجع رسمی. [Runs](https://docs.langchain.com/langsmith/runs).
72. LangChain — مستندات و مرجع رسمی. [Double texting](https://docs.langchain.com/langsmith/double-texting).
73. LangChain — مستندات و مرجع رسمی. [Webhooks](https://docs.langchain.com/langsmith/use-webhooks).
74. LangChain — مستندات و مرجع رسمی. [Backward compatibility](https://docs.langchain.com/oss/python/langgraph/backward-compatibility).
75. LangChain — مستندات و مرجع رسمی. [MCP](https://docs.langchain.com/oss/python/langchain/mcp).
76. LangChain — مستندات و مرجع رسمی. [MCP connections](https://docs.langchain.com/oss/python/langchain/mcp/connections).
77. LangChain — مستندات و مرجع رسمی. [MCP migration](https://docs.langchain.com/oss/python/migrate/langchain-mcp-adapters).
78. LangChain — مستندات و مرجع رسمی. [Agent Server API](https://docs.langchain.com/langsmith/server-api-ref).
79. LangChain — مستندات و مرجع رسمی. [Event streaming](https://docs.langchain.com/oss/python/langgraph/event-streaming).
80. LangChain — مستندات و مرجع رسمی. [Runtime](https://docs.langchain.com/oss/python/langchain/runtime).
81. LangChain — مستندات و مرجع رسمی. [Deep Agents overview](https://docs.langchain.com/oss/python/deepagents/overview).
82. LangChain — مستندات و مرجع رسمی. [Integration maintenance note](https://docs.langchain.com/oss/python/integrations/embeddings/instruct_embeddings).
