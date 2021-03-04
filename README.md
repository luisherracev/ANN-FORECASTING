I currently have this code in which the target to be predicted are the daily returns shown by the "rsp" variable. The other ones are the predictors.

    rsp <- dailyReturn(sp_adjusted)
    rsplag <- stats::lag(rsp,k=1)
    spopen <- stats::lag(sp_open,k=1)
    sphigh <- stats::lag(sp_high,k=1)
    splow <- stats::lag(sp_low,k=1)
    spclose <- stats::lag(sp_close,k=1)
    # dataframe
    rspall <- cbind(rsp, rsplag, spopen, sphigh, splow, spclose)
    colnames(rspall) <- c('rsp','rsplag', 'spopen', 'sphigh', 'splow', 'spclose')
    rspall <- na.exclude(rspall)

Then I made the neural network:

    ann_train <- neuralnet(rsp~rsplag+spopen+sphigh+splow+spclose, data=rsp_train, hidden=3,
    act.fct="tanh")
    ann_train$result.matrix
    plot(ann_train)

And obtained this plot:

[Plot of the neural network with returns][1]


It makes sense to me that the output is -2.5 because it is predicting returns.

However, I'm trying to predict index stock PRICES, but when I change the target and predictor code without the dailyReturn()

    sp <- sp_adjusted
    splag <- stats::lag(rsp,k=1)
    spopen <- stats::lag(sp_open,k=1)
    sphigh <- stats::lag(sp_high,k=1)
    splow <- stats::lag(sp_low,k=1)
    spclose <- stats::lag(sp_close,k=1)
    # dataframe
    rspall <- cbind(sp, splag, spopen, sphigh, splow, spclose)
    colnames(rspall) <- c('sp','splag', 'spopen', 'sphigh', 'splow', 'spclose')
    rspall <- na.exclude(rspall)

The plot of the neural network is quite different, the output is 341.149, still not close to the actual price of the index, but not a return either, so I'm not sure if the NN is working. Besides, the error increases a lot.

[Plot of the NN w/o the daily returns][2]


In addition, when the predictions with or without the daily returns are made, the actual results  are quite weird. They remain the same through time. To be clear, I used years 2000-2016 as training set and 2017-2020 for testing.

    Predict=neuralnet::compute(ann_train,rsp_test)
    forecast1 <- as.data.frame(Predict$net.result)
    as.data.frame(Predict$net.result)

[Prediction data frame with neural networks][3]


  [1]: https://i.stack.imgur.com/Zfis9.png
  [2]: https://i.stack.imgur.com/KuzU7.png
  [3]: https://i.stack.imgur.com/8xh3L.png

I also tried using this code to form the neural network but it didn't change much. I still have the same problem, because it lies on the target and predictors. When making the predictions I also obtain the same result for everyday of the testing set, but instead of 1376.107, it is always 1.

    nn_train <- neuralnet(sp~splag+spopen+sphigh+splow+spclose,
                data = rsp_train,
                hidden = c(3,3),
                rep = 10,
                algorithm = "rprop-",
                act.fct = 'logistic',
                err.fct = 'sse',
                linear.output = FALSE)
    plot(nn_train, rep = "best")

I would really appreciate any help as I am a bit desperate, since it is a very important assignment for university I have to turn in in a few weeks! If there's anyone willing to further help me please let me know where I can contact you! 
