## House Prices - Advanced Regression Techniques

**პროექტის მიმოხილვა**

პროექტი არის Kaggle-ის 'შეჯიბრი', რომლის მიზანიც არის საცხოვრებელი სახლების ფასების პროგნოზირება სხვადასხვა მახასიათებლებსა და პარამეტრებზე დაყრდნობით. 
პრობლემის გადასაჭრელად გამოვიყენე: 
-მონაცემთა 'გასუფთავება' (Cleaning) და ზედმეტი ინფორმაციის მოშორება გრაფიკებზე დაყრდნობით (outliers)
-გამოტოვებული/ცარიელი უჯრების (NaN Values) შესაბამისად დამუშავება
-Feature Engineering (სხვადასხვა ტიპის ცვლადების დამუშავება და მათი განცალკევება)
-Feature Selection (კორელაციის მატრიცის დახმარებით ინფორმაციის გასუფთავება)
-სხვადასხვა მოდელის ტრენინგი და მათი ოპტიმიზაცია
-შედეგების ანალიზი

**რეპოზიტორიის სტრუქტურა**
-model_experiment.ipynb - ძირითადი Jupyter Notebook, სადაც დამუშავებულია მონაცემები, შესრულებულია Feature Engineering/Selection, შერჩეული და დატრენინგებულია მოდელები 
-model_inference.ipynb - Notebook, რომელიც იყენებს საუკეთესო მოდელს ტესტურ მონაცემებზე პროგნოზის გასაკეთებლად და Kaggle-ზე წარსადგენად
-README.md - პროექტის დოკუმენტაცია (მიმდინარე ფაილი)


**Feature Engineering**
კატეგორიული ცვლადების რიცხვითში გადაყვანა

კატეგორიული ცვლადები დავამუშავე რამდენიმე მეთოდით:
-One-hot კოდირება გამოვიყენე ნომინალური კატეგორიული ცვლადებისთვის
-Label კოდირება - ორდინალური კატეგორიული ცვლადებისთვის
-Target კოდირება - იმ კატეგორიული ცვლადებისთვის, რომლებსაც ბევრი უნიკალური მნიშვნელობა აქვთ 

რაც შეეხება, Nan მნიშვნელობების დამუშავებას, წავშალე ისეთი სვეტები, რომლებშიც უმეტესად NaN მნიშვნელობები იყო, რადგან ისინი ნაკლებად სასარგებლო იყო მოდელისთვის. დანარჩენ სვეტებში კი NaN მნიშვნელობები ჩავანაცვლე საშუალო, მედიანური მნიშვნელობებით ან 0-ით. კატეგორიული ცვლადების NaN მნიშვნელობები ჩავანაცვლე უხშირესი მნიშვნელობით ან სპეციფიკური კატეგორიით "Unknown/No"

Cleaning მიდგომები - მონაცემთა გასუფთავების ეტაპზე გავფილტრე outlier-ები, რომლებიც ავარჩიე გრაფიკიდან. წავშალე მაღალი უნიკალურობის მქონე სვეტები, როგორიცაა Id, რომლებიც არ შეიტანდა წვლილს პროგნოზირების სიზუსტეში და გავაერთიანე რამდენიმე მსგავსი მახასიათებელი.


**Feature Selecion**
გამოყენებული მიდგომები და მათი შეფასება:
-კორელაციის ანალიზი - კორელაციის მატრიცის ანალიზის შემდეგ წავშალე ისეთი სვეტები, რომლებიც ძლიერად იყვნენ დამოკიდებული სხვა სვეტებზე.


**Training**
გამოვცადე რამდენიმე მოდელი:

-Linear Regression Model
-Polynomial Regression 
-XGBoost

ჰიპერპარამეტრების ოპტიმიზაციისთვის გამოვიყენე:
-GridSearchCV - სისტემატური ძიება პარამეტრების გრიდზე, რაც კარგად გამოვიყენე პოლინომიალურ რეგრესიაში
-Cross-validation - მოდელის სტაბილურობის შესაფასებლად
-ჰიპერპარამეტრების სხვადასხვა კომბინაციების შემოწმება XGBoost-ისთვის (learning_rate, max_depth, n_estimators)

საბოლოო მოდელის შერჩევა:
საბოლოო მოდელად შევარჩიე XGBoost ცხადი მიზეზების გამო:
-ჰქონდა საუკეთესო მეტრიკები მოდელებს შორის: R² = 0.8641, RMSE = 0.1397, MAE = 0.0914
-აჯობა იყო როგორც პოლინომიალურ რეგრესიას, ასევე GridSearch-ით ოპტიმიზირებულ წრფივი რეგრესიასაც


**MLflow Tracking**
ყველა ექსპერიმენტი დალოგილია DagsHub-ზე და ხელმისაწვდომია შემდეგ ბმულებზე:
1. https://dagshub.com/jgushiann/ML-House_Pricing.mlflow/#/experiments/0?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D
  Model Performance:
    R²: -447716886457770560.0000 | Adjusted R²: -1396682026232392960.0000
    MAE: 32282024.4448 | RMSE: 253658361.5701 | MSE: 64342564394443408.0000
    AIC: 11536.46 | BIC: 12250.74
    F-statistic: 95.9092 | p-value: 1.1102e-16
გვაქვს underfitting

2. https://dagshub.com/jgushiann/ML-House_Pricing.mlflow/#/experiments/1?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D
  Polynomial Regression Results:
    Best parameters: {'poly_features__degree': 2, 'regression__fit_intercept': True}
    Test RMSE: 0.2025
    Test R²: 0.7147
    Test Adjusted R²: 0.7117
    Test MAE: 0.1316
    F-statistic: 237.1862 (p-value: 0.0000)
    AIC: -913.9496
    BIC: -902.9608
    Cross-validation mean score: 0.8534 (±0.0108)
უკეთესია, ვიდრე linear regression model
   
4. https://dagshub.com/jgushiann/ML-House_Pricing.mlflow/#/experiments/2?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D
  Model Evaluation Metrics:
    Mean Squared Error (MSE): 0.019529582554352984
    Root Mean Squared Error (RMSE): 0.13974828283150023
    Mean Absolute Error (MAE): 0.09137621632558456
    R-squared (R2): 0.8641066923995647
    Explained Variance: 0.865202561477084
    Mean Absolute Percentage Error (MAPE): 0.00766901247704431
    Median Absolute Error: 0.06173243514525151
    Best Hyperparameters: {'learning_rate': 0.1, 'max_depth': 3, 'n_estimators': 200}
გვაქვს დაბალი RMSE, MAE, MAPE და შედარებით მაღალი  R2, Explained Variance რაც მიუთითებს იმაზე, რომ მოდელი კარგად ერგება მონაცემებს.

6. https://dagshub.com/jgushiann/ML-House_Pricing.mlflow/#/experiments/3?searchFilter=&orderByKey=attributes.start_time&orderByAsc=false&startTime=ALL&lifecycleFilter=Active&modelVersionFilter=All+Runs&datasetsFilter=W10%3D

Model Registry - https://dagshub.com/jgushiann/ML-House_Pricing.mlflow/#/models







