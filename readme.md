Climate Change Project
Data Acquired From: https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data
Goal: Use time series to predict weather going into the future

def get_denver():
    if os.path.exists('denver.csv'):
        df = pd.read_csv('denver.csv',index_col=0)
    else:
        df = pd.read_csv('GlobalLandTemperaturesByCity.csv')
        df = df[df.City=='Denver']
        df.to_csv('denver.csv')
    return df

def prep_df(df):
    
    df.dt = pd.to_datetime(df.dt)
    df = df.set_index('dt')
    df = df.sort_index()
    df = df[['AverageTemperature']]
    df.columns = df.columns=['avg_temp']
    df['month'] = df.index.month
    df['year'] = df.index.year
    df = df[df.year>1834]
    return df

def split_data(df,bound1=.7,bound2=.9):

    train = df[:int(df.shape[0]*bound1)]
    val = df[int(df.shape[0]*bound1):int(df.shape[0]*bound2)]
    test = df[int(df.shape[0]*bound2):]
    return train, val, test

def plot_preds(yhat):
    plt.figure(figsize=(20,8))
    plt.ylabel('Avg. Temperature')
    plt.xlabel('Date')
    train.avg_temp.plot()
    val.avg_temp.plot()
    yhat.avg_temp.plot()

def add_rmse(model,rmse):
    df = pd.DataFrame({'model':[model],'rmse':[rmse]})
    return pd.concat([df_rmse,df])