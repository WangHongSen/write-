2018年10月8日
17:25

# 常用数据结构
## pd.Series(data,index=index)
	1. data可以是dict,np.narray,scalar；data内部数据可以不一致；
	2. 默认没有index;index可以是non-unique；
	3. 切片操作同np.narray,可使用布尔条件取得索引，可以直接用index还有get(index,default)方法
	4. 字典key自动转换成index
	5. name 属性命名，rename（）属性更新
## pd.DataFrame(data,index,columns)
	1. data可以是dict of 1D ndarrays, lists ,dicts. or series,2-Dnp.ndarray,structured or record ndarray,  a pd.Series,another DataFrame;
    2. index,columns大小与data匹配
	3. columns选择 df[column],  delete df.pop(column), add df[newColumns]=在ipython中可以使用tab自动补全
	4. 行列标签自动对齐，结果对象是原来行列标签的union；columns上align，row broadcasting（注，当包含time Series时在列上broadcasting）
	5. df.T转置（或者df.transpose()），可使用np中的数学函数
	6. df.types显示每一列的数据类型，df.index,df.columns,df.values分别显示df的行标签,列标签，数据内容
	7. df.describe()返回数据的统计信息（数值类型列）
	8. df.sort_index(axis=,ascending=) df.sort_values(by=' ',ascending=False)
	9. 获得column df[column], row切片[]，row选择df.loc[row,columns]
	10. 切片通过df.loc[],df.iloc[]实现，以及boolean；isin()实现filter（列上）
	11. 使用df.at(row,column)对某一个元素的修改（同样用df.iat()只使用数值坐标）;修改一行一列使用df.loc
	12. df.reindex(index,column) 重组织df；df.dropna(how=),how 可以为any或者all；df.fillna(values=)替换np.nan
	13. df.apply(function) 在每一列上执行函数
	14. df.concat() merging 行
	15. join pd.merge(one,two,on=columns)  SQL style列
	16. df.append()
	17. df.groupby(columns).操作()实现splitting,applying,combining
	18. df.stack()压缩df，结果很奇怪
	19. pivot table pd.pivot_table(df, values=,index=,columns=)重组表的内容
	
## pd.Panel  deprecated
## time Series
    1. pd.date_range(str,periods=,freq=),str可以是'20180101','1/1/2019'等合法形式；periods为数字，表征长度；freq可以是'D','S','M','W','Y','Min'
    2. 转换time zone, 生成pd.Series后使用series.tz_loacalize(),series.tz_convert()(需localize)，serize.to_period()(去掉freq后的时间信息)
## category
    1. 可以使用df[column].astype(type)转换为category
    2. 使用df[column].cat.categories新增category
    3. 使用df[column].cat.set_categories(list)修改
    4. df中的category列可以当做普通列进行sort_values,groupby
# 其他
    1. 画图df要先调用df.cumsum(), df.plot() ,使用plt.show()绘制图片
    2. 使用df.to_csv(path),df.read_csv(path,type)输入输出
    3. HDF5读取输入 df.to_hdf(path,dtype),pd.read_hdf(path,dtype)