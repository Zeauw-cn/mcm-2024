table(:,1)=Wimbledonfeaturedmatches(:,1)
table(:,2)=Wimbledonfeaturedmatches(:,2)
table(:,3)=Wimbledonfeaturedmatches(:,3)
table(:,4)=Wimbledonfeaturedmatches(:,4)
table(:,5:11)=Wimbledonfeaturedmatches(:,5:11)
table.Properties.VariableNames{'Var2'}='player1'
table.Properties.VariableNames{'Var3'}='player2'
table.Properties.VariableNames{'Var4'}='elapsed_time'
table.Properties.VariableNames{'Var5'}='set_no'
table.Properties.VariableNames{'Var6'}='game_no'
table.Properties.VariableNames{'Var7'}='point_no'
table.Properties.VariableNames{'Var8'}='p1_sets'
table.Properties.VariableNames{'Var9'}='p2_sets'
table.Properties.VariableNames{'Var10'}='p1_games'
table.Properties.VariableNames{'Var11'}='p2_games'
time=table.elapsed_time
timeseconds=3600*hour(time)+60*minute(time)+second(time)
table1=addvars(table,timeseconds)
j=0
for i=1:7283
    if Wimbledonfeaturedmatches.match_id(i)~=Wimbledonfeaturedmatches.match_id(i+1)
        j=j+1
        matchcut(j)=i
    end
end
timeduration=5%%可以通过修改这个改变计算的时间区间
for i=timeduration+1:7284
    spendtime(i)=0
    spendtime(i)=timeseconds(i)-timeseconds(i-timeduration)
end%%有些人的比赛数据没有时间
for i=timeduration+1:7284
    if spendtime(i)<0||spendtime(i)>1000
       spendtime(i)=0
    end
end%%将不合理的经历时间数据设置为0
table1(:,13)=array2table(spendtime.')
table1.Properties.VariableNames{'Var13'}='spendtimeinlastfewshots'
%%将过去几球的数据粘贴上去
for i=timeduration+1:7284
    p1serveno(i)=0
    p2serveno(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.server(j)==1
            p1serveno(i)=p1serveno(i)+1
        else
            p2serveno(i)=p2serveno(i)+1
        end
    end
end%%算过去几个球里面两个人分别发了几个球
table6=addvars(table1,p1serveno.')
table7=addvars(table6,p2serveno.')
%%进行结算
for i=timeduration+1:7284
    p1aceno(i)=0
    p2aceno(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.p1_ace(j)==1
            p1aceno(i)=p1aceno(i)+1
        end
        if Wimbledonfeaturedmatches.p2_ace(j)==1
            p2aceno(i)=p2aceno(i)+1
        end
    end
end%%算算过去几个球中两个人发了几个ace球
table12=addvars(table7,p1aceno.')
table13=addvars(table12,p2aceno.')
table13.Properties.VariableNames{'Var14'}='p1serveno'
table13.Properties.VariableNames{'Var15'}='p2serveno'
table13.Properties.VariableNames{'Var16'}='p1aceno'
table13.Properties.VariableNames{'Var17'}='p2aceno'
%%结算
temp1=table13
for i=timeduration+1:7284
    p1firstserveinnum(i)=0
    p2firstserveinnum(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.server(j)==1&&Wimbledonfeaturedmatches.serve_no(j)==1
            p1firstserveinnum(i)=p1firstserveinnum(i)+1
        end
        if Wimbledonfeaturedmatches.server(j)==2&&Wimbledonfeaturedmatches.serve_no(j)==1
            p2firstserveinnum(i)=p2firstserveinnum(i)+1
        end
    end
end%%算算过去几个球中两个人的一发进了多少个
Table44=addvars(temp1,p1firstserveinnum.')
Table55=addvars(Table44,p2firstserveinnum.')
Table55.Properties.VariableNames{'Var18'}='p1firstserveinnum'
Table55.Properties.VariableNames{'Var19'}='p2firstserveinnum'
temp2=Table55%%结算
for i=timeduration+1:7284
    p1doublefaultnum(i)=0
    p2doublefaultnum(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.server(j)==1&&Wimbledonfeaturedmatches.p1_double_fault(j)==1
            p1doublefaultnum(i)=p1doublefaultnum(i)+1
        end
        if Wimbledonfeaturedmatches.server(j)==2&&Wimbledonfeaturedmatches.p2_double_fault(j)==1
            p2doublefaultnum(i)=p2doublefaultnum(i)+1
        end
    end
end%%算算过去几个球中两人分别有几个双误
Table6=addvars(temp2,p1doublefaultnum.')
Table7=addvars(Table6,p2doublefaultnum.')
Table7.Properties.VariableNames{'Var20'}='p1doublefaultnum'
Table7.Properties.VariableNames{'Var21'}='p2doublefaultnum'
temp3=Table7%%结算
for i=timeduration+1:7284
    p1winnernum(i)=0
    p2winnernum(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.p1_winner(j)==1
            p1winnernum(i)=p1winnernum(i)+1
        end
        if Wimbledonfeaturedmatches.p2_winner(j)==1
            p2winnernum(i)=p2winnernum(i)+1
        end
    end
end%%算算过去几个球中两人分别有几个制胜分
Table1=addvars(temp3,p1winnernum.')
Table2=addvars(Table1,p2winnernum.')
Table2.Properties.VariableNames{'Var22'}='p1winnernum'
Table2.Properties.VariableNames{'Var23'}='p2winnernum'
temp4=Table2%%结算
for i=timeduration+1:7284
    p1netnum(i)=0
    p2netnum(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.p1_net_pt(j)==1
            p1netnum(i)=p1netnum(i)+1
        end
        if Wimbledonfeaturedmatches.p2_net_pt(j)==1
            p2netnum(i)=p2netnum(i)+1
        end
    end
end%%算算过去几个球中两人分别有几次来到网前


Table5=addvars(temp4,p1netnum.')
Table6=addvars(Table5,p2netnum.')
Table6.Properties.VariableNames{'Var24'}='p1netnum'
Table6.Properties.VariableNames{'Var25'}='p2netnum'
temp5=Table6%%结算
for i=timeduration+1:7284
    p1netwinnernum(i)=0
    p2netwinnernum(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.p1_net_pt_won(j)==1
            p1netwinnernum(i)=p1netwinnernum(i)+1
        end
        if Wimbledonfeaturedmatches.p2_net_pt_won(j)==1
            p2netwinnernum(i)=p2netwinnernum(i)+1
        end
    end
end%%算算过去几个球中两人分别有几次网前得分
Table7=addvars(temp5,p1netwinnernum.')
Table8=addvars(Table7,p2netwinnernum.')
Table8.Properties.VariableNames{'Var26'}='p1netwinnernum'
Table8.Properties.VariableNames{'Var27'}='p2netwinnernum'
temp6=Table8%%结算
for i=timeduration+1:7284
    p1unforcederrornum(i)=0
    p2unforcederrornum(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.p1_unf_err(j)==1
            p1unforcederrornum(i)=p1unforcederrornum(i)+1
        end
        if Wimbledonfeaturedmatches.p2_unf_err(j)==1
            p2unforcederrornum(i)=p2unforcederrornum(i)+1
        end
    end
end%%算算过去几个球中两人分别有几次非受迫失误
Table9=addvars(temp6,p1unforcederrornum.')
Table10=addvars(Table9,p2unforcederrornum.')
Table10.Properties.VariableNames{'Var28'}='p1unforcederrornum'
Table10.Properties.VariableNames{'Var29'}='p2unforcederrornum'
temp7=Table10%%结算
for i=timeduration+1:7284
    p1rundistance(i)=0
    p2rundistance(i)=0
    for j=i-timeduration:i-1
        p1rundistance(i)=p1rundistance(i)+Wimbledonfeaturedmatches.p1_distance_run(j)
        p2rundistance(i)=p2rundistance(i)+Wimbledonfeaturedmatches.p2_distance_run(j)
    end
end%%算算过去几个球中两人总共的跑动距离
Table11=addvars(temp7,p1rundistance.')
Table12=addvars(Table11,p2rundistance.')
Table12.Properties.VariableNames{'Var30'}='p1rundistance'
Table12.Properties.VariableNames{'Var31'}='p2rundistance'
temp8=Table12%%结算
for i=timeduration+1:7284
    totalrallylength(i)=0
    for j=i-timeduration:i-1
        totalrallylength(i)=totalrallylength(i)+Wimbledonfeaturedmatches.rally_count(j)
    end
end%%算算过去几个球中两人总共的回合数
Table12=addvars(temp8,totalrallylength.')
Table12.Properties.VariableNames{'Var32'}='totalrallylengthinlastfewpoints'
temp9=Table12%%结算
for i=timeduration+1:7284
    p1breakpointnum(i)=0
    p2breakpointnum(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.p1_break_pt(j)==1
            p1breakpointnum(i)=p1breakpointnum(i)+1
        end
        if Wimbledonfeaturedmatches.p2_break_pt(j)==1
            p2breakpointnum(i)=p2breakpointnum(i)+1
        end
    end
end%%算算过去几个球中两人分别有几个破发点
Table13=addvars(temp9,p1breakpointnum.')
Table14=addvars(Table13,p2breakpointnum.')
Table14.Properties.VariableNames{'Var33'}='p1breakpointnum'
Table14.Properties.VariableNames{'Var34'}='p2breakpointnum'
temp10=Table14%%结算
for i=timeduration+1:7284
    p1breakpointwinnum(i)=0
    p2breakpointwinnum(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.p1_break_pt_won(j)==1
            p1breakpointwinnum(i)=p1breakpointwinnum(i)+1
        end
        if Wimbledonfeaturedmatches.p2_break_pt_won(j)==1
            p2breakpointwinnum(i)=p2breakpointwinnum(i)+1
        end
    end
end%%算算过去几个球中两人分别赢了几个破发点
Table15=addvars(temp10,p1breakpointwinnum.')
Table16=addvars(Table15,p2breakpointwinnum.')
Table16.Properties.VariableNames{'Var35'}='p1breakpointwinnum'
Table16.Properties.VariableNames{'Var36'}='p2breakpointwinnum'
temp11=Table16%%结算
for i=timeduration+1:7284
    p1breakpointlosenum(i)=0
    p2breakpointlosenum(i)=0
    for j=i-timeduration:i-1
        if Wimbledonfeaturedmatches.p1_break_pt_missed(j)==1
            p1breakpointlosenum(i)=p1breakpointlosenum(i)+1
        end
        if Wimbledonfeaturedmatches.p2_break_pt_missed(j)==1
            p2breakpointlosenum(i)=p2breakpointlosenum(i)+1
        end
    end
end%%算算过去几个球中两人分别miss了几个破发点
Table17=addvars(temp11,p1breakpointlosenum.')
Table18=addvars(Table17,p2breakpointlosenum.')
Table18.Properties.VariableNames{'Var37'}='p1breakpointlosenum'
Table18.Properties.VariableNames{'Var38'}='p2breakpointlosenum'
temp12=Table18%%结算
writetable(temp12,'feature.csv')%%导出为csv文件
%%第一部分结束
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



%%第1.5部分，使用滑动5球内的发球速度平均值填充数据

modifiedservespeed=fillmissing(Wimbledonfeaturedmatches.speed_mph,'movmean',5)
modifiedelapsedtimeseconds=fillmissing(feature5.spendtimeinlastfewshots,'movmean',100)
changedtotaltime=fillmissing(feature5.timeseconds(503:635),'linear','SamplePoints',feature5.point_no(503:635));
for i=1:7284
    if i>=585&&i<=636
        Changedtotaltime(i)=changedtotaltime(i-584)+3262;
    else
        Changedtotaltime(i)=feature5.timeseconds(i);
    end
end
feature5result.timeseconds=Changedtotaltime.';








%%第二部分开始
timeduration=5
timeduration=5;
for i=1:7284
    if Wimbledonfeaturedmatches.serve_width(i)=='B'
        if Wimbledonfeaturedmatches.server(i)==1
        p1servewidth(i)=0;
        p2servewidth(i)=0;
        end
        if Wimbledonfeaturedmatches.server(i)==2
        p1servewidth(i)=0;
        p2servewidth(i)=0;
        end
    end
    if (Wimbledonfeaturedmatches.serve_width(i)=='BC')||(Wimbledonfeaturedmatches.serve_width(i)=='BW')
        if Wimbledonfeaturedmatches.server(i)==1
        p1servewidth(i)=1;
        p2servewidth(i)=-1;
        end
        if Wimbledonfeaturedmatches.server(i)==2
        p1servewidth(i)=-1;
        p2servewidth(i)=1;
        end
    end
    if Wimbledonfeaturedmatches.serve_width(i)=='C'||Wimbledonfeaturedmatches.serve_width(i)=='W'
        if Wimbledonfeaturedmatches.server(i)==1
        p1servewidth(i)=2;
        p2servewidth(i)=-2;
        end
        if Wimbledonfeaturedmatches.server(i)==2
        p1servewidth(i)=-2;
        p2servewidth(i)=2;
        end
    end
end%%以0，1，2标志发球的角度
W1=addvars(Wimbledonfeaturedmatches,p1servewidth.');
W2=addvars(W1,p2servewidth.');
W2.Properties.VariableNames{'Var47'}='p1servewidth';
W2.Properties.VariableNames{'Var48'}='p2servewidth';%%重命名
for i=1:7284
    if Wimbledonfeaturedmatches.serve_depth(i)=='CTL'
        if Wimbledonfeaturedmatches.server(i)==1
        p1servedepth(i)=1;
        p2servedepth(i)=-1;
        end
        if Wimbledonfeaturedmatches.server(i)==2
        p1servedepth(i)=-1;
        p2servedepth(i)=1;
        end
    end
    if Wimbledonfeaturedmatches.serve_depth(i)=='NCTL'
        if Wimbledonfeaturedmatches.server(i)==1
        p1servedepth(i)=0;
        p2servedepth(i)=0;
        end
        if Wimbledonfeaturedmatches.server(i)==2
        p1servedepth(i)=0;
        p2servedepth(i)=0;
        end
    end
end%%用1和0标志发球的深度
W3=addvars(W2,p1servedepth.');
W4=addvars(W3,p2servedepth.');
W4.Properties.VariableNames{'Var49'}='p1servedepth';
W4.Properties.VariableNames{'Var50'}='p2servedepth';%%重命名
for i=1:7284
        if Wimbledonfeaturedmatches.server(i)==1
        p1servespeed(i)=modifiedservespeed(i);
        p2servespeed(i)=-modifiedservespeed(i);
        end
        if Wimbledonfeaturedmatches.server(i)==2
        p1servespeed(i)=-modifiedservespeed(i);
        p2servespeed(i)=modifiedservespeed(i);
        end
end%%发球为正，接发球为负
W5=addvars(W4,p1servespeed.');
W6=addvars(W5,p2servespeed.');
W6.Properties.VariableNames{'Var51'}='p1servespeed';
W6.Properties.VariableNames{'Var52'}='p2servespeed';%%重命名
for i=1:7284
    if Wimbledonfeaturedmatches.return_depth(i)=='D'
        if Wimbledonfeaturedmatches.server(i)==1
        p1returndepth(i)=-1;
        p2returndepth(i)=1;
        end
        if Wimbledonfeaturedmatches.server(i)==2
        p1returndepth(i)=1;
        p2returndepth(i)=-1;
        end
    end
    if Wimbledonfeaturedmatches.return_depth(i)=='ND'
        if Wimbledonfeaturedmatches.server(i)==1
        p1returndepth(i)=1;
        p2returndepth(i)=-1;
        end
        if Wimbledonfeaturedmatches.server(i)==2
        p1returndepth(i)=-1;
        p2returndepth(i)=1;
        end
    else
        p1returndepth(i)=1;
        p2returndepth(i)=-1;
    end%%将双误时双方的回球深度指数记为0
end%%回球深对接发球方有优势，回球浅对接发球方有优势
W7=addvars(W6,p1returndepth.');
W8=addvars(W7,p2returndepth.');
W8.Properties.VariableNames{'Var53'}='p1returndepth';
W8.Properties.VariableNames{'Var54'}='p2returndepth';
for i=timeduration+1:7284
    p1servewidthindex(i)=0;
    p2servewidthindex(i)=0;
    for j=i-timeduration:i-1
            p1servewidthindex(i)=p1servewidthindex(i)+W8.p1servewidth(j);
            p2servewidthindex(i)=p2servewidthindex(i)+W8.p2servewidth(j);
    end
end%%算算过去几个球中两人的发球宽度系数
Y1=addvars(feature5,p1servewidthindex.');%%这里的feature5可以任意改动
Y2=addvars(Y1,p2servewidthindex.');
Y2.Properties.VariableNames{'Var39'}='p1servewidthindex';
Y2.Properties.VariableNames{'Var40'}='p2servewidthindex';
temp1=Y2;%%结算
for i=timeduration+1:7284
    p1servedepthindex(i)=0;
    p2servedepthindex(i)=0;
    for j=i-timeduration:i-1
            p1servedepthindex(i)=p1servedepthindex(i)+W8.p1servedepth(j);
            p2servedepthindex(i)=p2servedepthindex(i)+W8.p2servedepth(j);
    end
end%%算算过去几个球中两人的发球深度系数
Y3=addvars(Y2,p1servedepthindex.');
Y4=addvars(Y3,p2servedepthindex.');
Y4.Properties.VariableNames{'Var41'}='p1servedepthindex';
Y4.Properties.VariableNames{'Var42'}='p2servedepthindex';
temp2=Y4;%%结算

for i=timeduration+1:7284
    p1servespeedindex(i)=0;
    p2servespeedindex(i)=0;
    for j=i-timeduration:i-1
            p1servespeedindex(i)=p1servespeedindex(i)+W8.p1servespeed(j);
            p2servespeedindex(i)=p2servespeedindex(i)+W8.p2servespeed(j);
    end
end%%算算过去几个球中两人的发球速度系数
Y5=addvars(Y4,p1servespeedindex.');
Y6=addvars(Y5,p2servespeedindex.');
Y6.Properties.VariableNames{'Var43'}='p1servespeedindex';
Y6.Properties.VariableNames{'Var44'}='p2servespeedindex';
temp3=Y6;%%结算
for i=timeduration+1:7284
    p1returndepthindex(i)=0;
    p2returndepthindex(i)=0;
    for j=i-timeduration:i-1
            p1returndepthindex(i)=p1returndepthindex(i)+W8.p1returndepth(j);
            p2returndepthindex(i)=p2returndepthindex(i)+W8.p2returndepth(j);
    end
end%%算算过去几个球中两人的发球速度系数
Y7=addvars(Y6,p1returndepthindex.');
Y8=addvars(Y7,p2returndepthindex.');
Y8.Properties.VariableNames{'Var45'}='p1returndepthindex';
Y8.Properties.VariableNames{'Var46'}='p2returndepthindex';
temp4=Y8;%%结算
feature5result=Y8;
feature5result.spendtimeinlastfewshots=modifiedelapsedtimeseconds5;

