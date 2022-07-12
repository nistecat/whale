//@version=5
//Credits:@Noldo - Whale Trading System
//        @rumpypumpydumpy - ALMA Ribbons
//        @QuantNomad - Elastic Volume Weighted Moving Average
// Composite Indicator, Provides a nice visual for Which way the wind is blowing and how capital is moving. Can see when smart money is buying up a big dip or of they seem to still be on the sidelines. 
//Created by taking QuantNomad's EVWMA and using that as input for a variation of rumpypumpydumpy's ALMA Ribbons. Each Ribbon had its sub ribbons averaged and then fed through the ta.rsi and the ta.mom functions. Signal line created from close value being fed through the ta. ema into the ta.rsi then ta.wma then ta.mom function.
//Why those in that order? No reason in particular just what I stumbled upon after many variations.
//I then layed Noldo's Whale Trading System overtop to view what "whales" were doing, giving us a good view of when capital is flowing into and out the asset.
indicator("Whale Momentum Waves", overlay=false)
tf     = input.timeframe('1D',title="Wave TimeFrame" )
Offset = input(.66,          title="ALMA Offset")
Sigma  = input(6,            title="ALMA Sigma ")
src    = request.security(syminfo.ticker,tf,input.source(defval=close, title="Source1"))

evwmaLength=input(32,title="EVWMA Length")
useCV=input.bool(false, title="Use Cumulative Volume")
renderBands=input.bool(false, title="Draw Envelope")
nbfs = useCV ? ta.cum(volume) : math.sum(volume, evwmaLength)
medianSrc= request.security(syminfo.tickerid,tf,close)
calc_evwma(price, evwmaLength, nb_floating_shares) =>
    data = 0.0
    data := (nz(data[1]) * (nb_floating_shares - volume)/nb_floating_shares) + (volume*price/nb_floating_shares)
    data

m=calc_evwma(medianSrc, evwmaLength, nbfs)
lengthTypeb = input.bool(title="Length Input type",defval=false)

A1Length  = lengthTypeb ? input.int(16,  title="ALMA 1  Length") : input.int(16,  title="ALMA Length")
A2Length  = lengthTypeb ? input.int(32,  title="ALMA 2  Length") : A1Length*2
A3Length  = lengthTypeb ? input.int(64,  title="ALMA 3  Length") : A1Length*3
A4Length  = lengthTypeb ? input.int(80,  title="ALMA 4  Length") : A1Length*4
A5Length  = lengthTypeb ? input.int(128, title="ALMA 5  Length") : A1Length*5
A6Length  = lengthTypeb ? input.int(160, title="ALMA 6  Length") : A1Length*6
A7Length  = lengthTypeb ? input.int(200, title="ALMA 7  Length") : A1Length*7
A8Length  = lengthTypeb ? input.int(256, title="ALMA 8  Length") : A1Length*8
A9Length  = lengthTypeb ? input.int(300, title="ALMA 9  Length") : A1Length*9
A10Length = lengthTypeb ? input.int(360, title="ALMA 10 Length") : A1Length*10
A11Length = lengthTypeb ? input.int(420, title="ALMA 11 Length") : A1Length*10


//RibbonRSILength=input.string("A3Length",title="Ribbon RSI Length",options=["A1Length", "A2Length","A3Length","A4Length", "A5Length","A6Length","A7Length", "A8Length","A9Length"])

rsiLength      = input.int(defval=64,title="Ribbon RSI Length")
momentumLength = input.int(defval=24,title="Momentum Length")
transplev      = input.int(defval=30,title="Transparency Level 1",  minval=0, maxval=100)
transplev2     = input.int(defval=75,title="Transparency Level 2",  minval=0, maxval=100)
transplev3     = input.int(defval=10,title="Transparency Level 3",  minval=0, maxval=100)

curve1   = ta.alma(m,A1Length                                 ,Offset,Sigma)
curve1a  = ta.alma(m,A1Length + (A2Length  - A1Length)   / 5  ,Offset,Sigma)   
curve1b  = ta.alma(m,A1Length + (A2Length  - A1Length)   / 5*2,Offset,Sigma)
curve1c  = ta.alma(m,A1Length + (A2Length  - A1Length)   / 5*3,Offset,Sigma)
curve1d  = ta.alma(m,A1Length + (A2Length  - A1Length)   / 5*4,Offset,Sigma)
curve1e  = ta.alma(m,A1Length + (A2Length  - A1Length)   / 5*5,Offset,Sigma)
curve1f  = ta.alma(m,A1Length + (A2Length  - A1Length)   / 5*6,Offset,Sigma)
curve2   = ta.alma(m,A2Length                                 ,Offset,Sigma)
curve2a  = ta.alma(m,A2Length + (A3Length  - A2Length)   / 5  ,Offset,Sigma) 
curve2b  = ta.alma(m,A2Length + (A3Length  - A2Length)   / 5*2,Offset,Sigma)
curve2c  = ta.alma(m,A2Length + (A3Length  - A2Length)   / 5*3,Offset,Sigma)
curve2d  = ta.alma(m,A2Length + (A3Length  - A2Length)   / 5*4,Offset,Sigma)
curve2e  = ta.alma(m,A2Length + (A3Length  - A2Length)   / 5*5,Offset,Sigma)
curve2f  = ta.alma(m,A2Length + (A3Length  - A2Length)   / 5*6,Offset,Sigma)
curve3   = ta.alma(m,A3Length                                 ,Offset,Sigma)
curve3a  = ta.alma(m,A3Length + (A4Length  - A3Length)   / 5  ,Offset,Sigma)
curve3b  = ta.alma(m,A3Length + (A4Length  - A3Length)   / 5*2,Offset,Sigma)
curve3c  = ta.alma(m,A3Length + (A4Length  - A3Length)   / 5*3,Offset,Sigma)
curve3d  = ta.alma(m,A3Length + (A4Length  - A3Length)   / 5*4,Offset,Sigma)
curve3e  = ta.alma(m,A3Length + (A4Length  - A3Length)   / 5*5,Offset,Sigma)
curve3f  = ta.alma(m,A3Length + (A4Length  - A3Length)   / 5*6,Offset,Sigma)
curve4   = ta.alma(m,A4Length                                 ,Offset,Sigma)
curve4a  = ta.alma(m,A4Length + (A5Length  - A4Length)   / 5  ,Offset,Sigma)
curve4b  = ta.alma(m,A4Length + (A5Length  - A4Length)   / 5*2,Offset,Sigma)
curve4c  = ta.alma(m,A4Length + (A5Length  - A4Length)   / 5*3,Offset,Sigma)
curve4d  = ta.alma(m,A4Length + (A5Length  - A4Length)   / 5*4,Offset,Sigma)
curve4e  = ta.alma(m,A4Length + (A5Length  - A4Length)   / 5*5,Offset,Sigma)
curve4f  = ta.alma(m,A4Length + (A5Length  - A4Length)   / 5*6,Offset,Sigma)
curve5   = ta.alma(m,A5Length                                 ,Offset,Sigma)
curve5a  = ta.alma(m,A5Length + (A6Length  - A5Length)   / 5  ,Offset,Sigma)  
curve5b  = ta.alma(m,A5Length + (A6Length  - A5Length)   / 5*2,Offset,Sigma)
curve5c  = ta.alma(m,A5Length + (A6Length  - A5Length)   / 5*3,Offset,Sigma)
curve5d  = ta.alma(m,A5Length + (A6Length  - A5Length)   / 5*4,Offset,Sigma)
curve5e  = ta.alma(m,A5Length + (A6Length  - A5Length)   / 5*5,Offset,Sigma)
curve5f  = ta.alma(m,A5Length + (A6Length  - A5Length)   / 5*6,Offset,Sigma)
curve6   = ta.alma(m,A6Length                                 ,Offset,Sigma)
curve6a  = ta.alma(m,A6Length + (A7Length  - A6Length)   / 5  ,Offset,Sigma)
curve6b  = ta.alma(m,A6Length + (A7Length  - A6Length)   / 5*2,Offset,Sigma)
curve6c  = ta.alma(m,A6Length + (A7Length  - A6Length)   / 5*3,Offset,Sigma)
curve6d  = ta.alma(m,A6Length + (A7Length  - A6Length)   / 5*4,Offset,Sigma)
curve6e  = ta.alma(m,A6Length + (A7Length  - A6Length)   / 5*5,Offset,Sigma)
curve6f  = ta.alma(m,A6Length + (A7Length  - A6Length)   / 5*6,Offset,Sigma)
curve7   = ta.alma(m,A7Length                                 ,Offset,Sigma)
curve7a  = ta.alma(m,A7Length + (A8Length  - A7Length)   / 5  ,Offset,Sigma)
curve7b  = ta.alma(m,A7Length + (A8Length  - A7Length)   / 5*2,Offset,Sigma)
curve7c  = ta.alma(m,A7Length + (A8Length  - A7Length)   / 5*3,Offset,Sigma)
curve7d  = ta.alma(m,A7Length + (A8Length  - A7Length)   / 5*4,Offset,Sigma)
curve7e  = ta.alma(m,A7Length + (A8Length  - A7Length)   / 5*5,Offset,Sigma)
curve7f  = ta.alma(m,A7Length + (A8Length  - A7Length)   / 5*6,Offset,Sigma)
curve8   = ta.alma(m,A8Length                                 ,Offset,Sigma)
curve8a  = ta.alma(m,A8Length + (A9Length  - A8Length)   / 5  ,Offset,Sigma)
curve8b  = ta.alma(m,A8Length + (A9Length  - A8Length)   / 5*2,Offset,Sigma)
curve8c  = ta.alma(m,A8Length + (A9Length  - A8Length)   / 5*3,Offset,Sigma)
curve8d  = ta.alma(m,A8Length + (A9Length  - A8Length)   / 5*4,Offset,Sigma)
curve8e  = ta.alma(m,A8Length + (A9Length  - A8Length)   / 5*5,Offset,Sigma)
curve8f  = ta.alma(m,A8Length + (A9Length  - A8Length)   / 5*6,Offset,Sigma)
curve9   = ta.alma(m,A9Length                                 ,Offset,Sigma)
curve9a  = ta.alma(m,A9Length + (A10Length - A9Length)   / 5  ,Offset,Sigma)
curve9b  = ta.alma(m,A9Length + (A10Length - A9Length)   / 5*2,Offset,Sigma)
curve9c  = ta.alma(m,A9Length + (A10Length - A9Length)   / 5*3,Offset,Sigma)
curve9d  = ta.alma(m,A9Length + (A10Length - A9Length)   / 5*4,Offset,Sigma)
curve9e  = ta.alma(m,A9Length + (A10Length - A9Length)   / 5*5,Offset,Sigma)
curve9f  = ta.alma(m,A9Length + (A10Length - A9Length)   / 5*6,Offset,Sigma)
curve10  = ta.alma(m,A10Length                                ,Offset,Sigma)
curve10a = ta.alma(m,A10Length + (A11Length - A10Length) / 5  ,Offset,Sigma)   
curve10b = ta.alma(m,A10Length + (A11Length - A10Length) / 5*2,Offset,Sigma)
curve10c = ta.alma(m,A10Length + (A11Length - A10Length) / 5*3,Offset,Sigma)
curve10d = ta.alma(m,A10Length + (A11Length - A10Length) / 5*4,Offset,Sigma)
curve10e = ta.alma(m,A10Length + (A11Length - A10Length) / 5*5,Offset,Sigma)
curve10f = ta.alma(m,A10Length + (A11Length - A10Length) / 5*6,Offset,Sigma)


av1      = ta.rsi((curve1 + curve1a + curve1b + curve1c + curve1d + curve1e) /6 ,rsiLength) 
av2      = ta.rsi((curve2 + curve2a + curve2b + curve2c + curve2d + curve2e) /6 ,rsiLength) 
av3      = ta.rsi((curve3 + curve3a + curve3b + curve3c + curve3d + curve3e) /6 ,rsiLength) 
av4      = ta.rsi((curve4 + curve4a + curve4b + curve4c + curve4d + curve4e) /6 ,rsiLength) 
av5      = ta.rsi((curve5 + curve5a + curve5b + curve5c + curve5d + curve5e) /6 ,rsiLength) 
av6      = ta.rsi((curve6 + curve6a + curve6b + curve6c + curve6d + curve6e) /6 ,rsiLength) 
av7      = ta.rsi((curve7 + curve7a + curve7b + curve7c + curve7d + curve7e) /6 ,rsiLength) 
av8      = ta.rsi((curve8 + curve8a + curve8b + curve8c + curve8d + curve8e) /6 ,rsiLength) 
av9      = ta.rsi((curve9 + curve9a + curve9b + curve9c + curve9d + curve9e) /6 ,rsiLength) 

wave1= ta.mom(av1,momentumLength)
wave2= ta.mom(av2,momentumLength)
wave3= ta.mom(av3,momentumLength)
wave4= ta.mom(av4,momentumLength)
wave5= ta.mom(av5,momentumLength)
wave6= ta.mom(av6,momentumLength)
wave7= ta.mom(av7,momentumLength)
wave8= ta.mom(av8,momentumLength)
wave9= ta.mom(av9,momentumLength)

rsiSignal= ta.rsi(ta.ema(close,32),64)
signal         = ta.mom(ta.wma(rsiSignal,momentumLength/3),momentumLength)
mommomentum_ma = ta.mom(ta.wma(m,rsiLength/2),momentumLength)



plotSignal = plot(signal,color=color.white,linewidth=1,style=plot.style_circles,transp=50)
plotav1 = plot(wave1,title= ' Wave 1',color=color.yellow,linewidth=1)
plotav2 = plot(wave2,title= ' Wave 2',color=color.orange,linewidth=1)
plotav3 = plot(wave3,title= ' Wave 3',color=color.red,linewidth=1)
plotav4 = plot(wave4,title= ' Wave 4',color=color.maroon,linewidth=1)
plotav5 = plot(wave5,title= ' Wave 5',color=color.purple,linewidth=1)
plotav6 = plot(wave6,title= ' Wave 6',color=#4106c2,linewidth=1)
plotav7 = plot(wave7,title= ' Wave 7',color=color.blue,linewidth=1)
plotav8 = plot(wave8,title= ' Wave 8',color=color.teal,linewidth=1)
plotav9 = plot(wave9,title= ' Wave 9',color=color.aqua,linewidth=1)
fill(plotav1,plotav2,color=color.new(color.yellow,85))
fill(plotav2,plotav3,color=color.new(color.orange,85))
fill(plotav3,plotav4,color=color.new(color.red,85))
fill(plotav4,plotav5,color=color.new(color.maroon,85))
fill(plotav5,plotav6,color=color.new(color.purple,85))
fill(plotav6,plotav7,color=color.new(#4106c2,85))
fill(plotav7,plotav8,color=color.new(color.blue,85))
fill(plotav8,plotav9,color=color.new(color.aqua,85))
h1 = hline(0  ,color=color.white,linewidth=2, linestyle=hline.style_solid )
h2 = hline(40 ,color=color.lime,linewidth=1,  linestyle=hline.style_dashed  )
h3 = hline(-40,color=color.maroon,linewidth=1,linestyle=hline.style_dashed)
h4 = hline(25 ,color=color.green,linewidth=1, linestyle=hline.style_dotted )
h5 = hline(-25,color=color.red,linewidth=1,   linestyle=hline.style_dotted   )



reverse = false
lights          = input.string(title="Barcolor I / 0 ? ", options=["ON", "OFF"], defval="OFF")

lengthMFI   = input.int(52) //  You can use variables instead of integer now !!
lengthStoch = input.int(52) //  You can use variables instead of integer now !!

smoothK = lengthMFI/4            //  You can use variables instead of integer now !!
smoothD = lengthStoch/4          //  You can use variables instead of integer now !!

OverSold   = 20
OverBought = 80


// Essential Functions
// Function Exponential Moving Average 


f_ema(_src, _length)=>
    _length_adjusted = _length < 1 ? 1 : _length
    _multiplier = 2 / (_length_adjusted + 1)
    _return  = 0.00
    _return := na(_return[1]) ? _src : ((_src - _return[1]) * _multiplier) + _return[1]


// FUNCTION HIGHEST AND LOWEST  ( All Efforts goes to RicardoSantos )

f_highest(_src, _length)=>
    _adjusted_length = _length < 1 ? 1 : _length
    _value = _src
    for _i = 0 to (_adjusted_length-1)
        _value := _src[_i] >= _value ? _src[_i] : _value
    _return = _value

f_lowest(_src, _length)=>
    _adjusted_length = _length < 1 ? 1 : _length
    _value = _src
    for _i = 0 to (_adjusted_length-1)
        _value := _src[_i] <= _value ? _src[_i] : _value
    _return = _value

// Function Sum

f_sum(_src , _length) => 

    _output  = 0.00 
    
    _length_adjusted = _length < 1 ? 1 : _length
    
    for i = 0 to _length_adjusted-1
        _output := _output + _src[i]


// Money Flow Index (MFI)


lower = 20 // You can use non integer or mutable variables too !!
upper = 80 // You can use non integer or mutable variables too !!


// MFI

upper_s = f_sum(volume * (ta.change(src) <= 0 ? 0 : src), lengthMFI)
lower_s = f_sum(volume * (ta.change(src) >= 0 ? 0 : src), lengthMFI)


//_mfi = ta.mfi(upper_s, lower_s)
_mfi = 100.0 - (100.0 / (1.0 + upper_s/ lower_s))


// Function Stochastic Oscillator


f_stoch(_src , _length) => 

    100 * (_src - f_lowest(f_lowest(_src ,1), _length)) / (f_highest(f_highest(_src , 1), _length) - f_lowest(f_lowest(_src ,1), _length))


// Definition : Variables K3 and D

k3 = f_ema(f_stoch(_mfi, lengthStoch), smoothK)
d = f_ema(k3, smoothD)

positive_condition = k3 > d 
negative_condition = k3 < d

hist = k3 - d 

// Conditions 

pos_zone = k3 > d 
neg_zone = k3 < d

pos_moment = ta.crossover(k3,d)
neg_moment = ta.crossunder(k3,d)

col_bg = pos_moment ? color.teal : neg_moment ? color.maroon : na


// Clouds (FILTER )

opthi = f_highest(src , lengthMFI)
optlo = f_lowest(src  , lengthMFI)

 

optmid   = (opthi + optlo) / 2

negcloud = src < optmid and neg_zone
poscloud = src > optmid and pos_zone

col_zone = pos_zone ? color.green : neg_zone ? color.red : na

// Dist Sell

sell_con = false 

hist_down = hist < hist[1]
bar_up   = (((close[0] - close[1]) / close[1])  * 100) > (0.2)

sell_con := (hist_down and bar_up and poscloud)

// Dist Buy 

buy_con   = false

hist_up   = hist > hist[1]
bar_down = (((close[0] - close[1]) / close[1])  * 100) < (-0.2)

buy_con := (hist_up and bar_down and negcloud)


adj_dist = (sell_con ? color.orange : buy_con ? color.blue : na )


col_columns = (pos_zone and  not sell_con ? color.green : pos_zone and sell_con ? color.orange : 
               neg_zone and  not buy_con  ? color.red   : neg_zone and buy_con  ? color.blue   : na )

pos_size = hist
zero = 0

// Plot data

plot(pos_size , style = plot.style_columns, linewidth = 2, color = col_columns , transp = 25,title = "Relativity")
bgcolor(col_bg , transp = 70 , title = "Background Color")
plot(pos_moment  ? zero :na, style=plot.style_cross, color=color.teal  , linewidth=4, transp=0 ,title = "Distributional Buy Block")
plot(neg_moment  ? zero :na, style=plot.style_cross, color=color.maroon, linewidth=4, transp=0 ,title = "Distributional Sell Block")

// Barcolor 

_lights = 0.00 


if (lights=="ON")

    _lights:= 1.00
    
if (lights=="OFF")

    _lights:= -1.00   


bcolor_on  = _lights ==  1.00
bcolor_off = _lights == -1.00

barcolor(sell_con and bcolor_on ? color.orange: buy_con and bcolor_on ? color.blue : na )

// Alerts 

alertcondition(buy_con  , title='Distributional Buy' , message='Distributional Buy')
alertcondition(sell_con , title='Distributional Sell', message='Distributional Sell')
alertcondition(pos_moment  , title='Positive area' , message='Long')
alertcondition(neg_moment  , title='Negative area' , message='Short')


