# **Authentication**: Session Cookie and CSRF Token

***Don't have to do if using FarmosBasev1 server***

[USER] = user login into farmos website<br>
[PASS] = pass login into farmos website<br>
[URL] = farmos web address { ex. 'https://example.com' }

1. `curl --cookie-jar farmOS-cookie.txt -d 'name=[USER]&pass=[PASS]&form_id=user_login' [URL]/user/login` <br>
Used: `curl --cookie-jar farmOS-cookie.txt -d 'name=[USER]&pass=[PASS]&form_id=user_login' https://farmtest.peritusag.com/user/login`

2. `TOKEN="$(curl --cookie farmOS-cookie.txt [URL]/restws/session/token)"`<br>
Used: `TOKEN="$(curl --cookie farmOS-cookie.txt https://farmtest.peritusag.com/restws/session/token)"`

After executing 1 and 2, use this line for authentication in `curl`: 

**[AUTH] = `--cookie farmOS-cookie.txt -H "X-CSRF-Token: ${TOKEN}"`**

# Requesting Records

**Endpoints**<br>
Put these at the end of the [URL]. { ex. `curl [AUTH] [URL]/farm.json` }
- Assets: /farm_asset.json
  - Plantings: /farm_asset.json?type=planting
  - Animals: /farm_asset.json?type=animal
  - Equipment: /farm_asset.json?type=equipment
  - ...
- Logs: /log.json
  - Activities: /log.json?type=farm_activity
  - Observations: /log.json?type=farm_observation
  - Harvests: /log.json?type=farm_harvest
  - Inputs: /log.json?type=farm_input
  - ...
- Taxonomy terms: /taxonomy_term.json
  - Areas*: /taxonomy_term.json?bundle=farm_areas
  - Quantity Units: /taxonomy_term.json?bundle=farm_quantity_units
  - Crops/varieties: /taxonomy_term.json?bundle=farm_crops
  - Animal species/breeds: /taxonomy_term.json?bundle=farm_animal_types
  - Log categories: /taxonomy_term.json?bundle=farm_log_categories
  - Seasons: /taxonomy_term.json?bundle=farm_season
  - ...

**List of Records**<br>
Code: `curl --cookie farmOS-cookie.txt -H "X-CSRF-Token: ${TOKEN}" https://farmtest.peritusag.com/log.json?type=farm_harvest`<br>
Output:
>{"self":"http:\/\/farmtest.peritusag.com\/log?type=farm_harvest","first":"http:\/\/farmtest.peritusag.com\/log?type=farm_harvest\u0026page=0","last":"http:\/\/farmtest.peritusag.com\/log?type=farm_harvest\u0026page=0","list":[{"inventory":[],"movement":{"area":[],"geometry":null},"quantity":[],"id":"463","name":"harve","type":"farm_harvest","uid":{"uri":"http:\/\/farmtest.peritusag.com\/user\/7","id":"7","resource":"user"},"timestamp":"1633007752","created":"1626751312","changed":"1626751374","done":"0","url":"http:\/\/farmtest.peritusag.com\/log\/463","files":[],"flags":[],"images":[],"data":null,"area":[{"uri":"http:\/\/farmtest.peritusag.com\/taxonomy_term\/77","id":"77","resource":"taxonomy_term","name":"\u6c9f\u5317\u519c\u573a"}],"asset":[],"geofield":[{"geom":"MULTILINESTRING ((117.11437455905 37.246557894949, 117.11437455905 37.247133581377, 117.11527854912 37.247133581377, 117.11523334962 37.244902771971, 117.11435195929 37.244848799958, 117.11437455905 37.245424499442, 117.11308637318 37.245424499442, 117.11310779898 37.246473977888, 117.11437455905 37.246557894949), (117.11427435077 37.244055282257, 117.11536233965 37.244017625306, 117.11532449655 37.243482894578, 117.11428381155 37.243573271869, 117.11427435077 37.244055282257), (117.11580402135 37.247905785605, 117.12095403507 37.247806942512, 117.11966221175 37.242490101708, 117.11584171299 37.242774551248, 117.11589668419 37.243168402685, 117.11823296041 37.243037119102, 117.11828793162 37.244174902562, 117.11575925618 37.244349944647, 117.11580402135 37.247905785605))","geo_type":"multilinestring","lat":"37.245182502772","lon":"117.116856923370","left":"117.113086373180","top":"37.247905785605","right":"117.120954035070","bottom":"37.242490101708","srid":null,"latlon":"37.245182502772,117.116856923370","schemaorg_shape":""}],"log_category":[],"log_owner":[{"uri":"http:\/\/farmtest.peritusag.com\/user\/7","id":"7","resource":"user"}],"notes":[],"equipment":[],"lot_number":"2"}]}
 
 **Individual Record**<br>
Code: `curl --cookie farmOS-cookie.txt -H "X-CSRF-Token: ${TOKEN}" https://farmtest.peritusag.com/log/1.json`<br>
Output:
>{"membership":{"group":[]},"inventory":[],"movement":{"area":[],"geometry":null},"quantity":[],"id":"1","name":"\u64ad\u79cd\u51c6\u5907\u5de5\u4f5c","type":"farm_activity","uid":{"uri":"http:\/\/farmtest.peritusag.com\/user\/1","id":"1","resource":"user"},"timestamp":"1617123600","created":"1614885393","changed":"1626145409","done":"0","url":"http:\/\/farmtest.peritusag.com\/log\/1","files":[],"flags":[],"images":[],"data":null,"area":[],"asset":[],"geofield":[],"log_category":[],"log_owner":[{"uri":"http:\/\/farmtest.peritusag.com\/user\/5","id":"5","resource":"user"}],"notes":{"value":"\u003Cp\u003E\u64ad\u79cd\u51c6\u5907\u5de5\u4f5c\u003C\/p\u003E\n","format":"farm_format"},"equipment":[]}

**Filtering**<br>
Code: `curl --cookie farmOS-cookie.txt -H "X-CSRF-Token: ${TOKEN}" https://farmtest.peritusag.com/log.json?done=1`
Output:
>{"self":"http:\/\/farmtest.peritusag.com\/log?done=1","first":"http:\/\/farmtest.peritusag.com\/log?done=1\u0026page=0","last":"http:\/\/farmtest.peritusag.com\/log?done=1\u0026page=3","next":"http:\/\/farmtest.peritusag.com\/log?done=1\u0026page=1","list":[{"membership":{"group":[]},"inventory":[],"movement":{"area":[],"geometry":null},"quantity":[],"id":"7","name":"\u65bd\u80a5","type":"farm_activity","uid":{"uri":"http:\/\/farmtest.peritusag.com\/user\/7","id":"7","resource":"user"},"timestamp":"1615410000","created":"1615510167","changed":"1616117347","done":"1","url":"http:\/\/farmtest.peritusag.com\/log\/7","files":[],"flags":[],"images":[],"data":null,"area":[{"uri":"http:\/\/farmtest.peritusag.com\/taxonomy_term\/13","id":"13","resource":"taxonomy_term","name":"\u4e50\u7ef4\u519c\u573a_GHJZHDD-01"}],"asset":[],"geofield":[{"geom":"POLYGON ((117.11580402135 37.247905785605, 117.12095403507 37.247806942512, 117.11966221175 37.242490101708, 117.11584171299 37.242774551248, 117.11589668419 37.243168402685, 117.11823296041 37.243037119102, 117.11828793162 37.244174902562, 117.11575925618 37.244349944647, 117.11580402135 37.247905785605))","geo_type":"polygon","lat":"37.245598827081","lon":"117.118227309740","left":"117.115759256180","top":"37.247905785605","right":"117.120954035070","bottom":"37.242490101708","srid":null,"latlon":"37.245598827081,117.118227309740","schemaorg_shape":"37.242490101708,117.115759256180 37.242490101708,117.120954035070 37.247905785605,117.120954035070 37.247905785605,117.115759256180 37.242490101708,117.115759256180"}],"log_category":[{"uri":"http:\/\/farmtest.peritusag.com\/taxonomy_term\/1","id":"1","resource":"taxonomy_term","name":"Plantings"}],"log_owner":[{"uri":"http:\/\/farmtest.peritusag.com\/user\/7","id":"7","resource":"user"}],"notes":{"value":"\u003Cp\u003E\u4e50\u7ef4\u519c\u573a1\u53f7\u5730\u5757\uff0c3\u670810\u65e5\u8fdb\u884c\u65bd\u80a5\u4f5c\u4e1a\uff0c\u9762\u79ef70\u4ea9\uff0c\u4f7f\u7528\u80a5\u659927\u888b\uff0850Kg\/\u888b\uff09\uff0c\u4e34\u65f6\u7528\u5de54\u4eba\uff0c\u65bd\u80a5\u4f5c\u4e1a\u8d1f\u8d23\u4eba\uff1a\u90a2\u4f20\u540c\u003C\/p\u003E\n","format":"farm_format"},"equipment":[]},{"membership":{"group":[]},"inventory":[],"movement":{"area":[],"geometry":null},"quantity":[],"id":"13","name":"\u673a\u68b0\u65bd\u80a5-\u8fd4\u9752\u80a5\u65bd\u80a5\uff08\u5c3f\u7d20\uff09","type":"farm_activity","uid":{"uri":"http:\/\/farmtest.peritusag.com\/user\/7","id":"7","resource":"user"},"timestamp":"1615212000","created":"1615994697","changed":"1615997532","done":"1","url":"http:\/\/farmtest.peritusag.com\/log\/13","files":[],"flags":["monitor","review"],"images":[{"file":{"uri":"http:\/\/farmtest.peritusag.com\/file\/6","id":"6","resource":"file"}}],"data":null,"area":[{"uri":"http:\/\/farmtest.peritusag.com\/taxonomy_term\/27","id":"27","resource":"taxonomy_term","name":"\u59dc\u5357\u519c\u573a_GHJLLCH05"}],"asset":[],"geofield":[{"geom":"LINESTRING (117.48773072908 37.25880464259, 117.49362704191 37.25896530429, 117.49357666123 37.256407491034, 117.48758428262 37.256560868324, 117.48773055613 37.258801992809)","geo_type":"linestring","lat":"37.257677542229","lon":"117.490684996620","left":"117.487584282620","top":"37.258965304290","right":"117.493627041910","bottom":"37.256407491034","srid":null,"latlon":"37.257677542229,117.490684996620","schemaorg_shape":"37.256407491034,117.487584282620 37.256407491034,117.493627041910 37.258965304290,117.493627041910 37.258965304290,117.487584282620"}],"log_category":[{"uri":"http:\/\/farmtest.peritusag.com\/taxonomy_term\/1","id":"1","resource":"taxonomy_term","name":"Plantings"}],"log_owner":[{"uri":"http:\/\/farmtest.peritusag.com\/user\/7","id":"7","resource":"user"}],"notes":{"value":"\u003Cp\u003E\u59dc\u5357\u519c\u573a_GHJLLCH05\u4eca\u65e5\u5f00\u59cb\u8fdb\u884c\u8fd4\u9752\u80a5\u65bd\u80a5\uff1a\u003Cbr \/\u003E\na.\u65bd\u80a5\u54c1\u79cd-\u5c3f\u7d20\uff08\u603b\u6c2e\u226546%\uff0c50\u516c\u65a4\u88c5\uff09;\u003Cbr \/\u003E\nb.\u65b9\u5f0f\u6807\u51c6-\u673a\u6492\u6bcf\u4ea940\u65a4\u5c3f\u7d20\uff1b\u003Cbr \/\u003E\nc.\u4eba\u673a\u7528\u5de5\uff1a\u6492\u80a5\u673a\u68b01\u53f0\uff0c\u62d6\u62c9\u673a\u624b\u8def\u4eae\uff08\u5916\u5305\u5de5\uff09\uff0c\u4ef7\u683c5\u5143\/\u4ea9\uff1b\u96c7\u4f63\u5316\u80a5\u642c\u8fd0\u5de52\u4eba\uff08\u5e26\u62d6\u62c9\u673a150\u5143\/\u4eba\/\u5929\uff09\uff0c\u96c7\u4f63\u8d75\u534e\u677e1\u4eba\u76d1\u7ba1\u6492\u80a5\u8fdb\u5c55\u548c\u8d28\u91cf\uff08150\u5143\/\u5929\uff09\u3002\u003C\/p\u003E\n\u003Cp\u003E\u65bd\u80a5\u8fc7\u7a0b\u4e2d\u5bf9\u65bd\u80a5\u91cf\u8fdb\u884c\u4e86\u6d4b\u7b97\uff0c\u5e73\u5747\u65bd\u80a5\u91cf\u4e3a39.5\u65a4\/\u4ea9\uff0c\u5316\u80a5\u7528\u91cf67\u888b\u3002\u003C\/p\u003E\n","format":"farm_format"},"equipment":[]},......

Code: `curl --cookie farmOS-cookie.txt -H "X-CSRF-Token: ${TOKEN}" https://farmtest.peritusag.com/log.json?asset=5`<br>
Output:
>{"self":"http:\/\/farmtest.peritusag.com\/log?field_farm_asset=5","first":"http:\/\/farmtest.peritusag.com\/log?field_farm_asset=5\u0026page=0","last":"http:\/\/farmtest.peritusag.com\/log?field_farm_asset=5\u0026page=0","list":[]}

