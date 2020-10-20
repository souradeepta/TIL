### flatten json to a list of map

The topic is flatten json to a list of map, there is a piece of json, for example, as follows:

```json
{
	"uuid": "abc",
	"properties": {
	"sessionName": "Test session name"
	"waypoints": [
		{"uuid": "def", "properties": {"latitude": 3}}
				]
		}
}
```

Convert it to List<Map>, in the map, uuid is the key and properties is the value. So the result should be like below

```json
[
{"Uuid": "abc", "properties": {"sessionName": "Test session name", "waypoints": ["def"]}},
{"Uuid": "def", "properties": {"latitude": 3}},
…
]
```

In fact, it is a recursive request. Every time you put an object into the map, you need to use a recursive function to process each attribute. If it is an array, it needs to be recursive. Put all the objects in the array into res, and recursively recurse each object.

```java
  public List<Map<String, Object>> flatternJSON(String jsonString) {

        JSONParser parser = new JSONParser();

        List<Map<String, Object>> res = new ArrayList<Map<String, Object>>();

        try {

            JSONObject jObj = (JSONObject)parser.parse(jsonString);

            Map<String, Object> map = new HashMap<String, Object>();

            String key = (String) jObj.get("uuid");

            Object value = jObj;

            map.put(key, jObj);

            recursiveProcess(jObj, res);

            return res;

        } catch (ParseException e) {

            // TODO Auto-generated catch block

            e.printStackTrace();

        }

        return null;

    }

     

    private void recursiveProcess(JSONObject jObj, List<Map<String, Object>>res) {

        Iterator iter = jObj.keySet().iterator();

        while(iter.hasNext()) {

             

            String key = (String) iter.next();

            Object obj = jObj.get(key);

            if (obj instanceof String) {

                continue;

            } else if (obj instanceof JSONArray) {

                JSONArray array = (JSONArray)obj;

                Iterator arrIter = array.iterator();

                JSONArray newArray = new JSONArray();

                while(arrIter.hasNext()) {

                    JSONObject o = (JSONObject) arrIter.next();

                    newArray.add(o.get("uuid"));

                    Map<String, Object> map = new HashMap<String, Object>();

                    map.put((String) o.get("uuid"), obj);

                    res.add(map);

                    recursiveProcess(o, res);

                }

                jObj.put(key, newArray);

                 

            } else {

                recursiveProcess((JSONObject)obj, res);

            }

        }

    }

}
```

