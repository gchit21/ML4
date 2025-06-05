# ML_HW4
დავალებად გვქონდა Kaggle-ის "Challenges in Representation Learning: Facial Expression Recognition Challenge" competition. მიზანი იყო შემდეგი: გადმოგვეცემოდა მასივი,
რომელშიც თითო მონაცემი იყო Integer, Grayscale image-ის პიქსელების შავ-თეთრობის მაჩვენებელი, 0 აღნიშნავდა შავს, ხოლო 255-თეთრს. ამ სურათებზე უნდა გაგვეწვრთნა მოდელი, რომელიც ამოიცნობდა, თუ რა facial expression ჰქონდა ამ grayscale სურათზე ადამიანს. 

# მიდგომა 

პირველ რიგში მოცემული მასივი გარდავქმენი ჯერ grayscale სურათში და მერე pytorch Tensor-ში. შემდეგ გავატარე ტრანსფორმერში, რომელშიც ვუკეთებდი შემდეგ რამეებს:
HorizontalFlip, RandomRotation და RandomCrop. ეს იყო მიმართული overfit-ის თავიდან ასაცილებლად, ნეირონებს რომ არ ესწავლათ ძალიან კონკრეტული feature-ები. 
შემდეგ მოდელს ვატრეინებ train-დატასეტზე, რომელიც ამოვიღე "icml_face_data.csv"-დან. validation-dataset-იც აქედან ამოვიღე, 'Usage' column-ის მეშვეობით და მაგის შედეგები ასახულია wandb-ს train_accuracy და Test_accuracy column-ებში. wandb-ზე და-log-ილი მაქვს მოდელების პარამეტრები,accuracy და loss-ი.

# მოდელები
2-Layer<br/>
რაც შეეხება მოდელებს, დავიწყე ყველაზე მარტივიდან 2-layer Cnn-დან და პარამეტრების ცვლილებით ვტესტავდი, რა საუკეთესო შედეგის მიღება შემეძლო. პარამეტრებად მქონდა:
learning_rate, drop_threshold და kernels[](რამდენი ნეირონი იყოს layer-ებზე).
link:https://wandb.ai/gchit21-free-university-of-tbilisi-/2-Layer?nw=nwusergchit21

3-Layer_Batchnorm<br/>
შემდეგ დავამატე კიდევ ერთი CNN Layer და დავურთე Batchnormalization, რადგან მხოლოდ 2-layer-იანი CNN დაბალ-mid feature-ებს სწავლობდა და მე-3-ს დამატებით
ფაქტობრივად მივეცი საშუალება უფრო high level feature-ების სწავლის. batchnorm-მა შეამცირა internal covariate shift, ასევე აუმჯობესებდა generalization-ს და უშველა vanishing gradients და overfit-ს.
link:https://wandb.ai/gchit21-free-university-of-tbilisi-/3-Layer_BatchNorm?nw=nwusergchit21

3-Layer_BatchNorm_Resnet<br/>
ზემოთა მოდეს დავუმატე Resnet-ის მსგავსება, Residual Block Layer-ი. წესიეთ ეს Residual  კავშირები ეხმარება deep model-ებს დეგრადაციის პრობლემის მოგვარებას, ქსელს საშუალებას აძლევს ისწავლოს residual mappings სრული ტრანსფორმაციების ნაცვლად. ამან ტრენინგი უფრო სტაბილური გახადა და მოდელს საშუალება მისცა ესწავლა უფრო საჭირო თვისებები overfit-ის ან ტრენინგის დროის ზრდის გარეშე.
link:https://wandb.ai/gchit21-free-university-of-tbilisi-/3-Layer_BatchNorm_Resnet?nw=nwusergchit21

5-Layer_CNN_LSTM_Resnet<br/>
აქ უკვე უბრალოდ ექსპერიმენტირება დავიწყე, მაინტერესებდა LSTM(Long-short term memory), რომელსაც LLM-ებში იყენებენ face expression recognition-ის მოდელში რომ ჩამემატებინა რა მოხდებოდა. მეგონა, რომ შესწავლილ feature-ებს(მაგალითად ) უკეთ შეაკავშირებდა ერთმანეთთან და მასე გამოიყვანდა შედეგს,თუ რა facial expression იყო სურთზე, მარა ფაქტობრივად არ გაუმჯობესებულა validation accuracy. შეიძლება ეს იმის ბრალიც იყოს რომ თან 2 CNN layer-იც დავამატე, და პარამეტრებზე რადგან უფრო მგრძნობიარე გახდა, ვერ შევარჩიე სათანადო პარამეტრები. რადგან გაუმჯობესება ვერ შევამჩნიე აქ შევჩერდი და Attentio/self attention მოდელი აღარ გავაკეთე.
link:https://wandb.ai/gchit21-free-university-of-tbilisi-/5-Layer_CNN_LSTM?nw=nwusergchit21

ყველა მოდელის run-ები wandb-ზე არის ასახული და საუკეთესო შედეგი დადო 3_layer_BatchNorm_Resnet-მა.


