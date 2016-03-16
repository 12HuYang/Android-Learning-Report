##和风天气api
###1.我的key:
	34e54ce6cef7419a9f1f5f616ab6ee61
###2.API地址:
	城市接口： https://api.heweather.com/x3/weather?cityid=城市ID&key=你的认证key
###3.参数说明:
	请求参数 			说明 															值
	city 		城市名称、支持中英文,不区分大小写和空格,城市和国家之间用英文逗号分割 	北京、beijing、london,united kingdom
	cityid 		城市ID,参见 国内城市ID列表 										CN101010100
	cityip 		城市IP,输入所在城市的任意IP 										123.45.67.8
	key 		用户认证key
###4.请求示例:
	https://api.heweather.com/x3/weather?cityid=CN101010100&key=34e54ce6cef7419a9f1f5f616ab6ee61
###5.返回数据示例:
	{
    "HeWeather data service 3.0": [
        {
            "aqi": {
                "city": {
                    "aqi": "150",
                    "co": "2",
                    "no2": "76",
                    "o3": "50",
                    "pm10": "143",
                    "pm25": "115",
                    "qlty": "轻度污染",
                    "so2": "50"
                }
            },
            "basic": {
                "city": "北京",
                "cnty": "中国",
                "id": "CN101010100",
                "lat": "39.904000",
                "lon": "116.391000",
                "update": {
                    "loc": "2016-03-12 17:50",
                    "utc": "2016-03-12 09:50"
                }
            },
            "daily_forecast": [
                {
                    "astro": {
                        "sr": "06:30",
                        "ss": "18:18"
                    },
                    "cond": {
                        "code_d": "101",
                        "code_n": "100",
                        "txt_d": "多云",
                        "txt_n": "晴"
                    },
                    "date": "2016-03-09",
                    "hum": "9",
                    "pcpn": "0.0",
                    "pop": "0",
                    "pres": "1034",
                    "tmp": {
                        "max": "9",
                        "min": "2"
                    },
                    "vis": "10",
                    "wind": {
                        "deg": "320",
                        "dir": "无持续风向",
                        "sc": "微风",
                        "spd": "4"
                    }
                },
                {
                    "astro": {
                        "sr": "06:28",
                        "ss": "18:19"
                    },
                    "cond": {
                        "code_d": "100",
                        "code_n": "100",
                        "txt_d": "晴",
                        "txt_n": "晴"
                    },
                    "date": "2016-03-10",
                    "hum": "14",
                    "pcpn": "0.0",
                    "pop": "0",
                    "pres": "1030",
                    "tmp": {
                        "max": "12",
                        "min": "-2"
                    },
                    "vis": "10",
                    "wind": {
                        "deg": "309",
                        "dir": "北风",
                        "sc": "3-4",
                        "spd": "10"
                    }
                },
                {
                    "astro": {
                        "sr": "06:26",
                        "ss": "18:20"
                    },
                    "cond": {
                        "code_d": "100",
                        "code_n": "101",
                        "txt_d": "晴",
                        "txt_n": "多云"
                    },
                    "date": "2016-03-11",
                    "hum": "12",
                    "pcpn": "0.0",
                    "pop": "0",
                    "pres": "1023",
                    "tmp": {
                        "max": "15",
                        "min": "4"
                    },
                    "vis": "10",
                    "wind": {
                        "deg": "201",
                        "dir": "无持续风向",
                        "sc": "微风",
                        "spd": "3"
                    }
                },
                {
                    "astro": {
                        "sr": "06:25",
                        "ss": "18:21"
                    },
                    "cond": {
                        "code_d": "101",
                        "code_n": "101",
                        "txt_d": "多云",
                        "txt_n": "多云"
                    },
                    "date": "2016-03-12",
                    "hum": "20",
                    "pcpn": "0.0",
                    "pop": "0",
                    "pres": "1015",
                    "tmp": {
                        "max": "15",
                        "min": "4"
                    },
                    "vis": "10",
                    "wind": {
                        "deg": "101",
                        "dir": "无持续风向",
                        "sc": "微风",
                        "spd": "6"
                    }
                },
                {
                    "astro": {
                        "sr": "06:23",
                        "ss": "18:22"
                    },
                    "cond": {
                        "code_d": "101",
                        "code_n": "101",
                        "txt_d": "多云",
                        "txt_n": "多云"
                    },
                    "date": "2016-03-13",
                    "hum": "12",
                    "pcpn": "0.0",
                    "pop": "0",
                    "pres": "1023",
                    "tmp": {
                        "max": "17",
                        "min": "6"
                    },
                    "vis": "10",
                    "wind": {
                        "deg": "338",
                        "dir": "无持续风向",
                        "sc": "微风",
                        "spd": "10"
                    }
                },
                {
                    "astro": {
                        "sr": "06:22",
                        "ss": "18:23"
                    },
                    "cond": {
                        "code_d": "101",
                        "code_n": "104",
                        "txt_d": "多云",
                        "txt_n": "阴"
                    },
                    "date": "2016-03-14",
                    "hum": "11",
                    "pcpn": "0.0",
                    "pop": "0",
                    "pres": "1018",
                    "tmp": {
                        "max": "18",
                        "min": "6"
                    },
                    "vis": "10",
                    "wind": {
                        "deg": "197",
                        "dir": "无持续风向",
                        "sc": "微风",
                        "spd": "3"
                    }
                },
                {
                    "astro": {
                        "sr": "06:20",
                        "ss": "18:24"
                    },
                    "cond": {
                        "code_d": "104",
                        "code_n": "100",
                        "txt_d": "阴",
                        "txt_n": "晴"
                    },
                    "date": "2016-03-15",
                    "hum": "12",
                    "pcpn": "0.0",
                    "pop": "0",
                    "pres": "1016",
                    "tmp": {
                        "max": "18",
                        "min": "6"
                    },
                    "vis": "10",
                    "wind": {
                        "deg": "125",
                        "dir": "无持续风向",
                        "sc": "微风",
                        "spd": "2"
                    }
                }
            ],
            "hourly_forecast": [
                {
                    "date": "2016-03-12 01:00",
                    "hum": "24",
                    "pop": "0",
                    "pres": "1021",
                    "tmp": "1",
                    "wind": {
                        "deg": "170",
                        "dir": "南风",
                        "sc": "微风",
                        "spd": "8"
                    }
                },
                {
                    "date": "2016-03-12 04:00",
                    "hum": "32",
                    "pop": "0",
                    "pres": "1020",
                    "tmp": "1",
                    "wind": {
                        "deg": "119",
                        "dir": "东南风",
                        "sc": "微风",
                        "spd": "6"
                    }
                },
                {
                    "date": "2016-03-12 07:00",
                    "hum": "37",
                    "pop": "0",
                    "pres": "1019",
                    "tmp": "1",
                    "wind": {
                        "deg": "67",
                        "dir": "东北风",
                        "sc": "微风",
                        "spd": "7"
                    }
                },
                {
                    "date": "2016-03-12 10:00",
                    "hum": "30",
                    "pop": "0",
                    "pres": "1018",
                    "tmp": "4",
                    "wind": {
                        "deg": "66",
                        "dir": "东北风",
                        "sc": "微风",
                        "spd": "8"
                    }
                },
                {
                    "date": "2016-03-12 13:00",
                    "hum": "22",
                    "pop": "0",
                    "pres": "1016",
                    "tmp": "9",
                    "wind": {
                        "deg": "91",
                        "dir": "东风",
                        "sc": "微风",
                        "spd": "6"
                    }
                },
                {
                    "date": "2016-03-12 16:00",
                    "hum": "19",
                    "pop": "0",
                    "pres": "1015",
                    "tmp": "11",
                    "wind": {
                        "deg": "118",
                        "dir": "东南风",
                        "sc": "微风",
                        "spd": "6"
                    }
                },
                {
                    "date": "2016-03-12 19:00",
                    "hum": "24",
                    "pop": "0",
                    "pres": "1016",
                    "tmp": "9",
                    "wind": {
                        "deg": "214",
                        "dir": "西南风",
                        "sc": "微风",
                        "spd": "4"
                    }
                },
                {
                    "date": "2016-03-12 22:00",
                    "hum": "22",
                    "pop": "0",
                    "pres": "1018",
                    "tmp": "6",
                    "wind": {
                        "deg": "303",
                        "dir": "西北风",
                        "sc": "微风",
                        "spd": "12"
                    }
                }
            ],
            "now": {
                "cond": {
                    "code": "101",
                    "txt": "多云"
                },
                "fl": "-8",
                "hum": "32",
                "pcpn": "0",
                "pres": "1035",
                "tmp": "9",
                "vis": "10",
                "wind": {
                    "deg": "340",
                    "dir": "西南风",
                    "sc": "3-4",
                    "spd": "10"
                }
            },
            "status": "ok",
            "suggestion": {
                "comf": {
                    "brf": "较舒适",
                    "txt": "白天天气晴好，早晚会感觉偏凉，午后舒适、宜人。"
                },
                "cw": {
                    "brf": "较适宜",
                    "txt": "较适宜洗车，未来一天无雨，风力较小，擦洗一新的汽车至少能保持一天。"
                },
                "drsg": {
                    "brf": "较冷",
                    "txt": "建议着厚外套加毛衣等服装。年老体弱者宜着大衣、呢外套加羊毛衫。"
                },
                "flu": {
                    "brf": "较易发",
                    "txt": "天气较凉，较易发生感冒，请适当增加衣服。体质较弱的朋友尤其应该注意防护。"
                },
                "sport": {
                    "brf": "较不宜",
                    "txt": "天气较好，但考虑天气寒冷，推荐您进行室内运动，户外运动时请注意保暖并做好准备活动。"
                },
                "trav": {
                    "brf": "适宜",
                    "txt": "天气较好，同时又有微风伴您一路同行。虽会让人感觉有点凉，但仍适宜旅游，可不要错过机会呦！"
                },
                "uv": {
                    "brf": "最弱",
                    "txt": "属弱紫外线辐射天气，无需特别防护。若长期在户外，建议涂擦SPF在8-12之间的防晒护肤品。"
                }
            }
        }
    ]
	}

###6.json数据格式整理:
	o-JSON
	a-HeWeather data service 3.0
	o-[0]
	o-aqi
	o-basic
	a-daily_forecast
	a-hourly_forecast
	o-now
	v-status : "ok"
	o-suggestion
	(注:a代表JsonArray,o代表JsonObject)
###7.字段说明:
####(1)basic 城市基本信息
		字段		说明
		city	城市名称
		id		城市ID
		cnty	国家名称
		lat		纬度
		lon		经度
		update	数据更新时间,24小时制
		loc		数据更新的当地时间
		utc		数据更新的UTC时间
####(2)aqi 空气质量指数*
		字段		说明
		city	城市数据
		aqi		空气质量指数
		pm25	PM2.5 1小时平均值(ug/m³)
		pm10	PM10 1小时平均值(ug/m³)
		so2		二氧化硫1小时平均值(ug/m³)
		no2		二氧化氮1小时平均值(ug/m³)
		co		一氧化碳1小时平均值(ug/m³)
		o3		臭氧1小时平均值(ug/m³)
		qlty	空气质量类别
####(3)suggestion 生活指数*
		字段		说明
		drsg	穿衣指数
		brf		简介
		txt		详情
		uv		紫外线指数
		brf		简介
		txt		详情
		cw		洗车指数
		brf		简介
		txt		详情
		trav	旅游指数
		brf		简介
		txt		详情
		flu		感冒指数
		brf		简介
		txt		详情
		sport	运动指数
		brf		简介
		txt		详情
####(4)alarms灾害预警*
		字段		说明
		title	标题
		type	类型
		level	级别
		stat	状态
		txt		描述
####(5)now 实况天气
		字段		说明
		tmp		当前温度(摄氏度)
		fl		体感温度
		wind	风力状况
		spd		风速(Kmph)
		sc		风力等级
		deg		风向(角度)
		dir		风向(方向)
		cond	天气状况
		code	天气代码
		txt		天气描述
		pcpn	降雨量(mm)
		hum		湿度(%)
		pres	气压
		vis		能见度(km)
####(6)daily_forecast 天气预报
		字段		说明
		date	当地日期
		astro	天文数值
		sr		日出时间
		ss		日落时间
		tmp		温度
		max		最高温度(摄氏度)
		min		最低温度(摄氏度)
		wind	风力状况
		spd		风速(Kmph)
		sc		风力等级
		deg		风向(角度)
		dir		风向(方向)
		cond	天气状况
		code_d	白天天气代码
		txt_d	白天天气描述
		code_n	夜间天气代码
		txt_n	夜间天气描述
		pcpn	降雨量(mm)
		pop		降水概率
		hum		湿度(%)
		pres	气压
		vis		能见度(km)
####(7)hourly_forecast 每小时天气预报
		字段		说明
		date	当地日期和时间
		tmp		当前温度(摄氏度)
		wind	风力状况
		spd		风速(Kmph)
		sc		风力等级
		deg		风向(角度)
		dir		风向(方向)
		pop		降水概率
		hum		湿度(%)
		pres	气压
####(8)error code 错误代码
		代码					说明
		ok					接口正常
		invalid key			错误的用户 key
		unknown city		未知城市
		no more requests	超过访问次数
		anr					服务无响应或超时
		permission denied	没有访问权限
		(代表仅限国内城市)
###8.java(android)请求示例:
	   1 	String httpUrl = "https://api.heweather.com/x3/weather?cityid=城市ID&key=XXXXXXXXX";
	   2 	String jsonResult = request(httpUrl);
	   3 	System.out.println(jsonResult);
	   4 	public static String request(String httpUrl) {
	   5 	BufferedReader reader = null;String result = null;StringBuffer sbf = new StringBuffer();
	   6 	try {
	   7 	URL url = new URL(httpUrl);
	   8 	HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
	   9 	connection.setRequestMethod("GET");
	   10 	connection.connect();
	   11 	InputStream is = connection.getInputStream();
	   12 	reader = new BufferedReader(new InputStreamReader(is, "UTF-8"));
	   13 	String strRead = null;
	   14 	while ((strRead = reader.readLine()) != null) {
	   15 	sbf.append(strRead); sbf.append("\r\n");
	   16 	}
	   17 	reader.close();
	   18 	result = sbf.toString();
	   19 	} catch (Exception e) { e.printStackTrace(); }
	   20 	return result;
	   21 	} 

###9.天气代码对照:
	代码	中文	英文	图标
	100	晴	Sunny/Clear	100.png
	101	多云	Cloudy	101.png
	102	少云	Few Clouds	102.png
	103	晴间多云	Partly Cloudy	103.png
	104	阴	Overcast	104.png
	200	有风	Windy	200.png
	201	平静	Calm	201.png
	202	微风	Light Breeze	202.png
	203	和风	Moderate/Gentle Breeze	203.png
	204	清风	Fresh Breeze	204.png
	205	强风/劲风	Strong Breeze	205.png
	206	疾风	High Wind, Near Gale	206.png
	207	大风	Gale	207.png
	208	烈风	Strong Gale	208.png
	209	风暴	Storm	209.png
	210	狂爆风	Violent Storm	210.png
	211	飓风	Hurricane	211.png
	212	龙卷风	Tornado	212.png
	213	热带风暴	Tropical Storm	213.png
	300	阵雨	Shower Rain	300.png
	301	强阵雨	Heavy Shower Rain	301.png
	302	雷阵雨	Thundershower	302.png
	303	强雷阵雨	Heavy Thunderstorm	303.png
	304	雷阵雨伴有冰雹	Hail	304.png
	305	小雨	Light Rain	305.png
	306	中雨	Moderate Rain	306.png
	307	大雨	Heavy Rain	307.png
	308	极端降雨	Extreme Rain	308.png
	309	毛毛雨/细雨	Drizzle Rain	309.png
	310	暴雨	Storm	310.png
	311	大暴雨	Heavy Storm	311.png
	312	特大暴雨	Severe Storm	312.png
	313	冻雨	Freezing Rain	313.png
	400	小雪	Light Snow	400.png
	401	中雪	Moderate Snow	401.png
	402	大雪	Heavy Snow	402.png
	403	暴雪	Snowstorm	403.png
	404	雨夹雪	Sleet	404.png
	405	雨雪天气	Rain And Snow	405.png
	406	阵雨夹雪	Shower Snow	406.png
	407	阵雪	Snow Flurry	407.png
	500	薄雾	Mist	500.png
	501	雾	Foggy	501.png
	502	霾	Haze	502.png
	503	扬沙	Sand	503.png
	504	浮尘	Dust	504.png
	506	火山灰	Volcanic Ash	506.png
	507	沙尘暴	Duststorm	507.png
	508	强沙尘暴	Sandstorm	508.png
	900	热	Hot	900.png
	901	冷	Cold	901.png
	999	未知	Unknown	999.png