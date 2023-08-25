# Speech-machine-analysis-for-diagnosing-cognitive-and-psychiatric-disorders---depression
A thesis submitted to the Graduate Studies Office In partial fulfillment of the requirements for The degree of Master's degree in Computer Engineering. University of Tehran  College of Engineering  Faculty of  Electrical and Computer Engineering

By:Mina Zarifi - Supervisors:Dr. A Vahabie ,Dr. BN Araabi - June, 2023 -
mina.zarifi@ut.ac.ir 



It should be noted that almost at the end of each section, the obtained data is saved and used in the next section. Therefore, you do not need to execute the cells in order. Instead, you can use any section you need with appropriate data.

The Python code file is saved as Zarifi_project.ipynb, and the explanations are stored in a .docx file. The remaining files are in CSV format, which contains the data obtained from each section. To run each code snippet, you can download the relevant section and use it.

Abstract:
    In this study, online data was collected from various adult subjects, which dataset included two different speech tasks. The first task involved answering three questions about the past, present, and future, while the second task involved describing eight images by the subjects. Additionally, four inventory named the Beck Depression Inventory, Beck Anxiety Inventory, Beck State-Trait Anxiety Inventory, and P.A.N.A.S. were also collected from the subjects.
The aim of the research was to study depression as a disease. To evaluate the symptoms of depression, the speech task of describing eight pictures was used by 119 random subjects over eighteen years of age, and the scores of individuals in the Beck Depression Inventory. This research has been carried out with the aim of analyzing the relationship between the scores of the Beck Depression Inventory and the characteristics of the text. Using semantic vector similarity analysis, it was shown that some of these features have an effect on depression symptoms. For example adults with more depression symptoms had a greater standard deviation of Euclidean and cosine distances between semantic vectors in sentences. In addition, while it was found that some features are effective in describing depression symptoms, a linear regression machine learning model was used to determine their importance in predicting these psychological states. The examined features include speech graph, text sentiments, jump of ideas, and semantic embedding vectors. Embedding vectors were also used to analyze the relationship between features and Beck Depression Inventory scores. The embedding vectors were obtained using three methods: token-based, sentence-based, and sentence-averaging embedding. All stages (finding the correlation between features and Beck Depression Inventory scores, observing similarity matrix for each image, predicting Beck Depression Inventory scores using a linear regression model) were performed for all three cases. They were also used to analyze the similarity matrix so that if a feature has a specific trend according to the Beck Depression Inventory  scores and the similarity matrix between semantic embedding vectors, that feature can be used as input for regressing and predicting the Beck Depression Inventory scores. 

1.Create only image CSV


Please note that if you have the CSV data file, you don't need to run this section. The dataset consists of audio test data from two speech tasks. One speech task involves three questions about the past, present, and future of the participants, and the second speech task involves participants talking about eight images. In this project, only the data from the second speech task, which is related to 8 images, is used. That's why the "Create only image CSV" section is performed in the code. In this section, the data is first converted from mp3 format to wav (this code only needs to be executed once). After that, speech to text conversion is performed using Vosk (the code cells of this section should be executed in order).


preproccess 


In the preprocess section, incomplete data was identified and manually completed by the participants by either deleting or filling in the missing parts (others do not need this section, and this part was done specifically for my project's data).

The transcripts obtained automatically through Vosk were examined in this section and the data was cleaned. There were eight images in the task. It was observed that those who spoke clearly had a very good transcript available. Those who mistakenly skipped speaking about one image due to a weak internet connection or other reasons and only one voice recording was not uploaded to the server were also included in the dataset, and their text was manually entered into the dataset. Some files were shuffled (the order of audio files related to images was mixed up), and they were corrected as well. Those who accidentally did not record the audio file for one image and spoke about the previous image in the next image were also manually corrected.

The duration of each audio was also included in the dataset.
The number of remaining individuals is 123. Only the data related to eight images is saved in one file and will be analyzed from now on. The dataset has a total of 984 rows, and every 8 rows correspond to one participant.



2.COMPUTE BDI



In the COMPUTE BDI section, the scores of the Beck Depression Inventory questionnaire are calculated. It should be noted that the scores of other Inventorys are calculated using different formulas, which you need to implement yourself. In this section, the BDI scores of individuals are computed and added to the dataset to be later used as labels. Individuals who have not completed the BDI or whose Inventory responses were not uploaded to the server due to internet speed issues were sent a message to complete it. The task then proceeds to determine the Inventorys that have been answered more than once.
The Inventorys that have been answered more than once are retained only for their first occurrence, and the rest of the responses are deleted. Individuals who manually changed their Inventory IDs were identified based on the time of completing the Inventory and the audio test, and they were added to the dataset. The dataset columns were also modified so that each row corresponds to an audio file. Every 8 rows belong to one person who spoke about 8 audio files. They are grouped in such a way that each group has only one ID in the new dataset, and the lengths of the conversations are summed, and the text of all audio files is merged together. The "first block: read images CSV and first BDI" and "second block: read only second BDI CSV" sections are related to calculating the BDI for a set of data initially. Later, when a new set of data is added, the BDI scores for the new data are calculated and added to the previous ones. If you also want to add new data to your dataset, this section will be useful for you.


3. categorization


This section is used when you want to use classification instead of regression. In this case, you will have 6 classes:

Total Score : Levels of Depression
1-10 : These ups and downs are considered normal
11-16 : Mild mood disturbance
17-20 : Borderline clinical depression
21-30 : Moderate depression
31-40 : Severe depression
over 40 : Extreme depression


groupby id

In the "groupby id" section, the data for each person, which consists of 8 rows, is merged into one row (all their words are placed side by side in relation to all images). The total length of each person's conversations is also added as a column called "total_length" to the data. In the "vectorized groupbied data" section, all the conversations of each person about each of the eight images are vectorized using ParsBERT. The vectorized dataset has dimensions (118, 773).
It should be noted that this part was used to test different methods, but it was not used anymore because the results were not satisfactory. Another solution was used, which will be mentioned later.

4. vectorized - analysis

   
در بخش vectorised with parsbert دادگان اصلی که شامل صحبت های افراد به تفکیک هر تصویر است با پارس برت تبدیل به بردار شده اند. 
! لازم به ذکر است که ادامه این بخش فقط برای تحلیل و آنالیز بیشتر و آشنا شدن با دادگان است و شما اصلا نیازی به اجرای آن ندارید. زیرا در یخش های بعد تحلیل هایی که نیاز هستند را انجام خواهید داد.
در بخش Similarit Marix از آن بردار ها استفاده شده برای رسم ماتریس شباهت. با توجه به اینکه ابعاد دادگان وکتورایز شده(768, 952) است ابعاد ماتریس شباهت  (952, 952) می باشد.similarity matrix برای وکتورها رسم شد. سپس بر اساس امتیاز BDI ، طول زمانی که افراد صحبت کرده اند، بر اساس ترتیب عکس ها، و بر اساس امتیازات BDI کتگوری شده، sort شد. 
یک سری آنالیز داده در این بخش انجام شده که در صورت دلخواه م یتوانید از آنها استفاده کنید. 

6. similarity matrix

   
با در دست داشتن دادگان وکتورایز شده که ویژگی های زبانی آن نیز در دیتاست وجود دارد میتوان ماتریس شباهت را به ترتیب ویژگی های مورد نیاز حتی به تفکیک تصاویر تحلیلی و بررسی کرد. این کار در ادامه انجام خواهد شد. 
میزان پرش فکری که آزمودنی­ها داشته‌اند نیز با استفاده از فاصله اقلیدسی و با استفاده از کسینوس زاویه بین بردار‌های معنایی تعبیه شده هر جمله در متون محاسبه شده و میانگین، انحراف معیار و مجموع فاصله‌ها به عنوان ویژگی استفاده شده‌اند. با داشتن n جمله در یک متن، n-1 معیار در آن متن خواهیم داشت. این به آن معنا است که برای متونی که فقط یک جمله دارند نمی‌توان این ویژگی را محاسبه کرد و مقدار nall به آن تعلق میگیرد.
در بخش sort by euclidean_mean feature and then show the similarity matrix همانطور که از اسمش پیداست ابتدا بر اساس میانگین اقلیدسی دادگان مرتب شده اند بعد میزان cosine_similarity بین بردارهای محاسبه شده است سپس ماتریس شباهت به دست آمده و رسم شده است.(دلیل کم شدن دیتا به 686در 686 این است که در دادگانی که تنها حاوی یک جمله بودند نمیتوان ویژگی های فاصله پرش فکری را محاسبه کرد و آنها حذف شده اند)
این عمل برای شش ویژگی فاصله پرش فکری انجام شده است. این 6 حالت شامل میانگین  و انحراف معیار و مجموع فواصل در هر دو حالت فاصله اقلیدسی و فاصله کسینوسی می باشد.
از میزان هم‌بستگی هر یک از ویژگی‌های به دست آمده با امتیازات  پرسش‌نامه افسردگی بک می‌توان به ویژگی‌های مهم دست یافت و در ادامه به آن‌ها پرداخت. حتی امتیازهای منفی ولی بزرگ نیز می‌توانند مهم باشند. این همبستگی زمانی که مقدارش از  دو تقسیم بر رادیکال n بیشتر باشد قابل قبول است. ویژگی‌های زیر روی 119 نفر که هر یک دارای صحبت در مورد توصیف هشت تصویر هستند می‌باشند. پس در نتیجه ویژگی‌های زیر برای 119*8=952 گفتار محاسبه شده‌اند. این به آن معنی است که همبستگی‌های بالای دو تقسیم بر رادیکال 952 که معادل است با مقدار 0.06 ، ارتباط معناداری با امتیازات پرسش‌نامه افسردگی بک دارند. مشاهده می‌شود که مقادیر میزان انحراف معیار و مجموع فاصله اقلیدسی و کسینوسی مقادیر قابل قبولی هستند. در بخش correlation میزان همبستگی هر یک از این ویژگی ها با میزان امتیازات پرسشنامه افسردگی بک محاسبه شده است که به شرح زیر است: 







8. scatter plot
   
در این بخش برای بیشتر آشنا شدن با دادگان همان ماتریس شباهت زمانی که به دو بعد کاهش بعد داده میشود رسم شده است.

10. unsupervised

این بخش زمانی استفاده میشود که شما بخواهید بدون استفاده از دادگان پرسشنامه فقط روی دادگان صوتی تحلیل و نتیجه گیری انجام دهید. به دلیل اینکه نتیجه خوبی در نهایت از این بخش گرفته نشد در نتیجه ی نهایی از آن استفاده نشده است. 
در این بخش پس از تبدیل داده های گروه بندی شده هر شخص(هر سطر مربوط به همه ی صحبت های یک شخص است و به تفکیک تصاویر جدا نشده اند) به بردار با استفاده از روش TF-IDF vectorization با روش kmeans و Hierarchical دادگان کلاستر میشوند.


11. feature extraction

    
پردازش زبان طبیعی یا NLP استخراج اطلاعات معنی دار از داده‌های زبان طبیعی و دستکاری ‌‌آن‌ها با استفاده از رایانه است. مدل‌های پردازش زبان طبیعی معمولا مبتنی بر زبان انگلیسی هستند. برای زبان‌های دیگر از مدل‌های چندزبانه با منابع محدود استفاده می‌شود. ‌پارس‌برت یک برت تک زبانه را برای زبان فارسی پیشنهاد می‌دهد. ‌پارس‌برت روی یک دیتای عمومی‌حجیم (Persian Wikidumps, MirasText) و داده‌های وبسایت‌های زیادی آموزش دیده شده. بخشی از متودولوژی ‌پارس‌برت این است که پیش‌پردازشی گسترده برای آوردن مجموعه داده‌ها به یک قالب مناسب انجام داده است. مدل ‌پارس‌برت به‌طور خاص بر روی واژگان خاص زبان با مقادیر انبوهی از داده‌های متن فارسی آموزش داده شده است. 
sentiment analysis

یکی از ویژگی‌های مورد استفاده، احساسات است. احساسات متن به دو روش محاسبه شده و از آن‌ها به عنوان ویژگی در ‌پیش‌بینی استفاده شده است. در روش اول یک تابع تعریف شده که با گرفتن یک رشته از کلمات به زبان فارسی با استفاده از کتابخانه hazm عملیات توکنیزه کردن و نشاندار کردن یک رشته کاراکتر به کوچکترین واحدی که در یک زبان قابل اهمیت است انجام می‌شود. بعد با استفاده از مدل طبقه بندی متن میزان امتیاز احساسات هر متن ‌پیش‌بینی می‌شود. در روش دوم تابع خط لوله از کتابخانه‌ترنسفورماتورها یک مدل یادگیری ماشین از پیش آموزش دیده شده را بارگزاری کرده و از آن برای تسک پردازش طبیعی تحلیل احساسات استفاده کرده است. احساسات عددی بین صفرو یک است که احتمال مثبت بودن یک متن را نشان میدهد. جایی که امتیاز احساسات نزدیک به یک باشد نشان دهنده احساسات مثبت‌تر و نزدیک به صفر نشان دهنده احساسات منفی‌تر است. اگر این میزان نزدیک به نیم باشد به این معنی است که مدل نمی‌تواند با اطمینان در مورد احساسات متن نظر دهد.

Speeh Graph Analysis

با استفاده از گراف گفتار که در آن هر کلمه به عنوان یک گره و ارتباط بین کلمات متوالی به عنوان یک یال نمایش داده می‌شوند یک سری ویژگی از متون استخراج شده است. ویژگی­های استخراج ­شده به شرح زیر هستند:
ویژگیهای زبانی شامل ویژگی‌های عمومی[1] مانند تعداد کل کلمات[2]، تعداد گره‌های گراف[3] و تعداد ارتباطات بین کلمات[4] می‌باشند. ویژگی‌های اجزاء متصل[5] مانند تعداد گره‌های بزرگترین جزئیات متصل یعنی بزرگترین زیرگراف که در آن همه کلمات با هم مرتبطند در گراف بدون جهت[6] و تعداد گره‌های بزرگترین جزئیات قوی متصل در گراف جهت دار[7] است.
ویژگی‌های بازگشتی[8] مانند مجموع تمام یالهای ساده یعنی لینک‌های تکرار شده[9] و لینک‌های موازی[10] که یک جفت گره را به هم مرتبط می‌کنند و ماتریس مجاورت بین کلمات[11] و نصف مجذور ماتریس مجاورت[12] هستند.
همچنین ویژگی‌های جهانی[13] مانند ، چگالی گراف[14]، قطر یا طول بزرگترین کوتاهترین مسیر بین در دو نود در گراف[15]، ضریب خوشه بندی[16]، متوسط کوتاه‌ترین مسیر بین هر دو نود در شبکه[17]، میانگین کل درجات[18] و .. محاسبه شده است.


[1] general attributes
[2] Word count
[3] nodes
[4] edges
[5] connected components
[6] Largest connected components
[7] Largest strongly connected components
[8] recurrence attributes
[9] Related edges
[10] Parallel edges
[11] L1
[12] L2
[13] global attributes
[14] density
[15] diameter
[16] clustering coefficient
[17] average shortest path
[18] average total degree


correlation

در این بخش میزان همبستگی هر یک از ویژگی های استخراج شده با BDI-score محاسبه شده است.از میزان هم‌بستگی هر یک از ویژگی‌های به دست آمده با امتیازات  پرسش‌نامه افسردگی بک می‌توان به ویژگی‌های مهم دست یافت و در ادامه به آن‌ها پرداخت. حتی امتیازهای منفی ولی بزرگ نیز می‌توانند مهم باشند. این همبستگی زمانی که مقدارش از  دو تقسیم بر رادیکال n بیشتر باشد قابل قبول است. ویژگی‌های زیر روی 119 نفر که هر یک دارای صحبت در مورد توصیف هشت تصویر هستند می‌باشند. پس در نتیجه ویژگی‌های زیر برای 119*8=952 گفتار محاسبه شده‌اند. این به آن معنی است که همبستگی‌های بالای دو تقسیم بر رادیکال 952 که معادل است با مقدار 0.06 ، ارتباط معناداری با امتیازات پرسش‌نامه افسردگی بک دارند. شکل زیر میزان همبستگی هر یک از سایر ویژگی‌ها با امتیازات پرسش‌نامه افسردگی بک را نشان میدهد. مشاهده می‌شود که ویژگی‌های PE، L1، L2، LCC، LSC، چگالی و قطر گراف گفتار و ASP ویژگی‌های قابل قبولی هستند که مقدار همبستگی آن‌ها با امتیازات پرسش‌نامه افسردگی بک از یک حدی بیشتر است.





ولی ما به این بسنده نکردیم و پس از آن به تفکیک هر تصویر میزان همبستگی ویژگی‌های پرش ذهنی با امتیازات پرسش‌نامه افسردگی بک محاسبه شده است. در این صورت می‌توان همسبتگی‌های جالبی بین ویژگی‌های درون هر تصویر به صورت خاص مشاهده کرد. در بخش‌های بعد این عمل با سه روش توکن‌محور، جمله‌محور و میانگین تعبیه جملات محور به تفکیک هر تصویر انجام شده و مقدار همبستگی و میزان قابل قبول بودن آن نیز به دست آورده ایم و در جداولی در ادامه نوشته شده است. 

10. SVM - RF- DT - balance - LOO


در این بخش سه روش svm و random forest و decision tree پیاده سازی شده و 
دادگان بالانس شده 
از روش leave one out استفاه شده.

TFIDF with 2 class

ابتدا در این بخش دادگان به دو کلاس نرمال و افسرده تقسیم شده که بین دو کلاس بالانس برقرار باشد و تعداد دادگان در یک کلاس خیلی کم یا خیلی زیاد نباشد. البته با این کار انگار یک سری اطلاعات دور ریخته میشوند زیرا میزان افسردگی دیگر در نظر گرفته نشده است. بعد روی بردارهایی که با TFIDF به دست آمده اند کاهش بعد انجام شده و بعد از پیاده سازی LeaveOneOut  روی مدل svm میزان معیارهای ارزیابی و Confusion Matrix و ROC Curve و Precision-Recall Curve نمایش داده شده اند.

Scatter Plot between features:

در این بخش اسکتر پلات بین ویژگی های مختلف به دست آمده تا ملموس تر بشد و بتوان تحلیل کرد تا ویژگی های نامناسب را حذف کرد.

TFIDF with 63 class _ balance

بالانس کردن در این بخش با تقسیم دادگان به دو کلاس انجام نمیشود بلکه با حذف دادگان از کلاسهایی که تعداد زیادی داده دارند انجام میشود تا در هر کلاس میزان یکسانی داده قرار داشته باشد. البته مشخص است که این روش ممکن است ممناسب نباشد و این مراحل فقط برای آزمایش راه حل های مختلف پیاده سازی شده بودند. که نتایج خوبی نداشتند.
visualized similarity matrix

در این بخش رسم ماتریس شباهت انجام شده است. درون صحبت های هر تصویر نیز به ترتیب BDI-score مرتب شده است. با استفاده از percentile میتوان ویژوالیزیشن بهتری از کاتریس شباهت داشت. 
به منظور تحلیل بیشتر از ماتریس شباهت استفاده شده تا تحلیل انجام شود. تحلیل و تفسیر دادگان در فضای ماتریس عدم تشابه بازنمایی[ representational dissimilarity matrix] انجام شده است تا شباهت تصاویر و ارتباط با امتیازات پرسش‌نامه افسردگی بک ملموس‌تر باشد. ماتریس شباهت از روی بردارهای به دست آمده برای هر متن بین گفتارها محاسبه شده است و بر اساس امتیازات پرسش‌نامه افسردگی بک مرتب سازی شده است تا بتوان ارتباطات معنادار را مشاهده کرد. این ماتریس بر اساس تصاویر به صورت مجزا نیز رسم شده است. رسم آن نیز بر حسب درصد[ percentile] می‌باشد تا بتوان درک بهتری از آن داشت. تبدیل یک ماتریس شباهت به یک ماتریس صدک می‌تواند برای درک بهتر و مقایسه شباهت بین مجموعه‌های مختلف داده مفید باشد. درصدها راهی برای رتبه‌بندی مقادیر نسبت به یکدیگر در یک توزیع فراهم می‌کنند که می‌تواند به شناسایی نقاط پرت یا الگوهایی در داده‌ها کمک کند که ممکن است بلافاصله با مشاهده نمرات شباهت خام آشکار نشوند.
با داشتن ماتریس تشابهی که نشان دهنده شباهت بین بردار‌های مختلف است، تبدیل آن به صدک می‌تواند کمک کند تا تشخیص دهید بردار‌های مربوط به کدام تصاویر به طور خاص به یکدیگر مرتبط هستند (یعنی مقادیر صدک بالایی دارند) و کدامیک به هم نامرتبط‌تر هستند. (یعنی مقادیر صدک کمتری دارند). این می‌تواند برای اهداف طبقه بندی یا برای شناسایی روابط تکاملی بین تصویر‌های مختلف مفید باشد.
در ماتریس مجاورت هر سطر و هر ستون نشان دهنده یک گفتار هستند. شباهت بردارهای معنایی سطر‌های ماتریس مجاورت با ستونهای ماتریس مجاورت، با رنگ درون هر سلول مشخص شده‌اند. قطر اصلی ماتریس مجاورت تماما یک می‌باشد به این معنی که هر گفتار با خودش بیشترین شباهت را دارد. ماتریس مجاورت یک ماتریس مربعی است که ابعاد آن به تعداد گفتارها در تعداد گفتار‌ها می‌باشد. میزان همبستگی بین ویژگی‌ها و امتیازات پرسش‌نامه افسردگی بک به صورت عادی و مرتب نشده در شکل های رسم شده (مانند شکل زیر) نمایش داده شده است.

هر یک از این بخش ها نیز میتواند از نزدیک تر نمایش داده شود که کد آن در بخش sub plots نوشته شده است. 


11. jump of idees & correlation:


هر شخص در مورد هشت تصویر صحبت کرده است. پس در ماتریس شباهت اطلاعات هر آزمودنی در واقع یک ماتریس هشت در هشت میباشد. عناصر قطر اصلی که شباهت هر گفتار با خودش را نشان میدهد همه یک هستند و عناصر بالا و پایین قطر اصلی نیز یکسان هستند. به همین دلیل تنها از 28 مقداری که در بالای قطر اصلی است استفاده شده است.ماتریس شباهت برای هر شخص به صورت مجزا که فقط از عناصر بالای قطر اصلی استفاده می‌شود چون عناصر زیر قطر اصلی تکراری هستند و  عناصر روی قطر اصلی تماما یک هستند.
وقتی که با اطلاعات ماتریس‌های  8 در 8 افراد  با ویژگی‌های مربوط به فاصله اقلیدسی و کسینوسی هر یک به صورت مجزا میزان امتیازات پرسش‌نامه افسردگی بک ‌پیش‌بینی می‌شود سه حالت داریم:
1.      حالت توکن‌محورtocken based embedding
2.      حالت جمله‌محور sentence based embedding
3.      حالت میانگین تعبیه جملات محور mean word based embedding
در هر یک از این سه حالت کارهای یکسانی انجام شده فقط نحوه تبدیل به بردار متفاوت است. در هر بخش بعد از به دست آوردن فواصل سینوسی و کسینوسی بردارهای معنایی ویژگی های مجموع و انحراف معیار و میانگین به دست آمده است، سپس میزان همبستگی هر ویژگی با BDI-score محاسبه شده است. مقدار significant در آن تعداد داده نیز محاسبه شده است و نمایش داده شده است.


12. percentile similarity matrix :


در این بخش برای سه حالت زیر رسم ماتریس مجاورت بر حسب درصد انجام شده است:
1.      حالت توکن‌محورtocken based embedding
2.      حالت جمله‌محور sentence based embedding
3.      حالت میانگین تعبیه جملات محور mean word based embedding
به منظور تحلیل بیشتر از ماتریس شباهت استفاده شده تا تحلیل انجام شود. تحلیل و تفسیر دادگان در فضای ماتریس عدم تشابه بازنمایی[ representational dissimilarity matrix] انجام شده است تا شباهت تصاویر و ارتباط با امتیازات پرسش‌نامه افسردگی بک ملموس‌تر باشد. ماتریس شباهت از روی بردارهای به دست آمده برای هر متن بین گفتارها محاسبه شده است و بر اساس امتیازات پرسش‌نامه افسردگی بک مرتب سازی شده است تا بتوان ارتباطات معنادار را مشاهده کرد. این ماتریس بر اساس تصاویر به صورت مجزا نیز رسم شده است. رسم آن نیز بر حسب درصد[ percentile] می‌باشد تا بتوان درک بهتری از آن داشت. تبدیل یک ماتریس شباهت به یک ماتریس صدک می‌تواند برای درک بهتر و مقایسه شباهت بین مجموعه‌های مختلف داده مفید باشد. درصدها راهی برای رتبه‌بندی مقادیر نسبت به یکدیگر در یک توزیع فراهم می‌کنند که می‌تواند به شناسایی نقاط پرت یا الگوهایی در داده‌ها کمک کند که ممکن است بلافاصله با مشاهده نمرات شباهت خام آشکار نشوند.
با داشتن ماتریس تشابهی که نشان دهنده شباهت بین بردار‌های مختلف است، تبدیل آن به صدک می‌تواند کمک کند تا تشخیص دهید بردار‌های مربوط به کدام تصاویر به طور خاص به یکدیگر مرتبط هستند (یعنی مقادیر صدک بالایی دارند) و کدامیک به هم نامرتبط‌تر هستند. (یعنی مقادیر صدک کمتری دارند). این می‌تواند برای اهداف طبقه بندی یا برای شناسایی روابط تکاملی بین تصویر‌های مختلف مفید باشد.
در ماتریس مجاورت هر سطر و هر ستون نشان دهنده یک گفتار هستند. شباهت بردارهای معنایی سطر‌های ماتریس مجاورت با ستونهای ماتریس مجاورت، با رنگ درون هر سلول مشخص شده‌اند. قطر اصلی ماتریس مجاورت تماما یک می‌باشد به این معنی که هر گفتار با خودش بیشترین شباهت را دارد. ماتریس مجاورت یک ماتریس مربعی است که ابعاد آن به تعداد گفتارها در تعداد گفتار‌ها می‌باشد.


13. prediction SVM:


لازم به ذکر است که این بخش برای تحلیل و آزمایش بیشتر انجام شده و نتایج اصلی در بخش بعد به دست آمده اند.
هر شخص در مورد هشت تصویر صحبت کرده است. پس در ماتریس شباهت اطلاعات هر آزمودنی در واقع یک ماتریس هشت در هشت میباشد و از همین به عنوان ویژگی برای مدل استفاده شده است. عناصر قطر اصلی که شباهت هر گفتار با خودش را نشان میدهد همه یک هستند و عناصر بالا و پایین قطر اصلی نیز یکسان هستند. به همین دلیل تنها از 28 مقداری که در بالای قطر اصلی است استفاده شده است.ماتریس شباهت برای هر شخص به صورت مجزا که فقط از عناصر بالای قطر اصلی استفاده می‌شود چون عناصر زیر قطر اصلی تکراری هستند و  عناصر روی قطر اصلی تماما یک هستند.
وقتی که با اطلاعات ماتریس‌های  8 در 8 افراد  با ویژگی‌های مربوط به فاصله اقلیدسی و کسینوسی هر یک به صورت مجزا میزان امتیازات پرسش‌نامه افسردگی بک ‌پیش‌بینی می‌شود سه حالت داریم:
1.      حالت توکن‌محورtocken based embedding
2.      حالت جمله‌محور sentence based embedding
3.      حالت میانگین تعبیه جملات محور mean word based embedding
در هر یک از این سه حالت کارهای یکسانی انجام شده فقط نحوه تبدیل به بردار متفاوت است

از ویژگی های فاصله کسینوسی و اقلیدسی به صورت مجزا برای پیش بینی استفاده شده است.(euclidean و cosine). هر یک از بخش های این قسمت بر اساس نامی که دارند مشخص است که چه پیاده سازی ای در آنها انجام شده است.


14. prediction : Linear regression


در این بخش از مدل رگرسیون خطی، برای ‌پیش‌بینی استفاده شده است و عملکرد ‌پیش‌بینی شدت افسردگی گزارش شده بر حسب ضریب تعیین[R-squared] و ریشه میانگین مربعات خطا[Root Mean Squared Error] ارزیابی شده است.
رگرسیون خطی یک روش آماری است که برای مدل‌سازی رابطه بین یک متغیر وابسته و یک یا چند متغیر مستقل استفاده می‌شود. هدف رگرسیون خطی یافتن بهترین خطی است که این رابطه را توصیف می‌کند و به ما امکان می‌دهد تا بر اساس مقادیر متغیرهای مستقل، در مورد متغیر وابسته پیش‌بینی کنیم. ایده اصلی پشت رگرسیون خطی این است که نقاط داده را روی یک نمودار پراکنده رسم کنیم و سپس یک خط مستقیم را در میان آن‌ها قرار دهیم که فاصله بین خط و نقاط داده را به حداقل برساند. سپس می‌توان از این خط برای پیش‌بینی مقادیر آینده متغیر وابسته بر اساس مقادیر جدید متغیرهای مستقل استفاده کرد. مدل‌های رگرسیون خطی در طیف وسیعی از کاربردها از جمله برای شناسایی روندها و روابط بین متغیرها استفاده می‌شوند که می‌تواند به تصمیم‌گیری در زمینه‌های مختلف کمک کند.
همانطور که در بخش های قبل گفته شده هر شخص در مورد هشت تصویر صحبت کرده است. پس در ماتریس شباهت اطلاعات هر آزمودنی در واقع یک ماتریس هشت در هشت میباشد و از همین به عنوان ویژگی برای مدل استفاده شده است. عناصر قطر اصلی که شباهت هر گفتار با خودش را نشان میدهد همه یک هستند و عناصر بالا و پایین قطر اصلی نیز یکسان هستند. به همین دلیل تنها از 28 مقداری که در بالای قطر اصلی است استفاده شده است.ماتریس شباهت برای هر شخص به صورت مجزا که فقط از عناصر بالای قطر اصلی استفاده می‌شود چون عناصر زیر قطر اصلی تکراری هستند و  عناصر روی قطر اصلی تماما یک هستند.
وقتی که با اطلاعات ماتریس‌های  8 در 8 افراد  با ویژگی‌های مربوط به فاصله اقلیدسی و کسینوسی هر یک به صورت مجزا میزان امتیازات پرسش‌نامه افسردگی بک ‌پیش‌بینی می‌شود سه حالت داریم:
1.      حالت توکن‌محورtocken based embedding از حالت tf-idf برای embedding استفاده شده است.
2.      حالت جمله‌محور sentence based embedding از حالت labse برای embedding استفاده شده است.
3.      حالت میانگین تعبیه جملات محور mean word based embedding از حالت parsbert برای embedding استفاده شده است.
برای هر یک از این سه حالت پیش بینی با روش Linear regression به تفکیک دو ویژگی (euclidean و cosine) انجام شده است. 











