##### Parse Nested JSON String | Technical Phone Interview

Small startup asked this question:



Given a JSON string and a list of strings, find all values in the JSON string corresponding to the keys given in the list of strings, and replace all numbers in the values with the '#' character.



e.g. {"user": {"age": "16 years", "name": "Bill Gates", "weight": "200 lbs", "height": "72 in"}, ["age", "height"]
output: {"user":{"age":"xx years", "name": "Bill Gates", "weight":"200 lbs", "height":"xx in"}



or more complicated case:
{"user":{"age":["16 years"], "name": "Bill Gates", "weight": {"weight": "200 lbs"}, "height":"72 in"}, ["age", "weight"]
output: {"user":{"age":["xx years"], "name": "Bill Gates", "weight": {"weight": "xxx lbs"}, "height":"72 in"}

```javascript
obj = {"user":{"age":"16 years", "name": "Bill Gates", "weight": {"weight": "200 lbs"}, "height":"72 in"}}
arr = ["age", "weight"]

let parseObj = (ob) =>{
		keys = Object.keys(ob);
		keys.map((key) => {
						if(typeof(ob[key]) === "object"){
											parseObj(ob[key])
						}else{
								if(arr.includes(key)){
										let value = ob[key];
										if(typeof(value) == "string"){
													let newarr = [...value].map(c => {
																if(c >="0" && c<="9")
																		return "#"
													return c
										})
								ob[key] = newarr.join("")
						}
						}
						}
	})
		return ob;
}
console.log(parseObj(obj))
```

```python
def parse(data, target: list) -> Dict:
    for key in data:
        value = data[key]
        if key in target and isinstance(value, str):
            data[key] = re.sub("\d", "x", value)
        if isinstance(value, dict):
            parse(value, target)
        elif isinstance(value, list):
            data[key] = list(map(lambda vv: re.sub("\d", "x", vv), value))
        
    return data
```



##### Print JSON format String

This was a recent interview question. Suppose we have given following string:



"{"id": "0001", "type": "donut","name": "Cake","ppu": 0.55, "batters":{"batter":[{ "id": "1001", "type": "Regular" },{ "id": "1002", "type": "Chocolate" }]},"topping":[{ "id": "5001", "type": "None" },{ "id": "5002", "type": "Glazed" }]}"
we need to print it in following order:



```json
{
	"id": "0001",
	"type": "donut",
	"name": "Cake",
	"ppu": 0.55,
	"batters":
		{
			"batter":
				[
					{ "id": "1001", "type": "Regular" },
					{ "id": "1002", "type": "Chocolate" }
				]
		},
	"topping":
		[
			{ "id": "5001", "type": "None" },
			{ "id": "5002", "type": "Glazed" }
		]
}
```

```java
public static void main(String[] args) {
    String json = "{\"id\": \"0001\", \"type\": \"donut\", \"name\": \"Cake\", \"ppu\": 0.55, \"batters\":{\"batter\":[{ \"id\": \"1001\", \"type\": \"Regular\" },{ \"id\": \"1002\", \"type\": \"Chocolate\" }]},\"topping\":[{ \"id\": \"5001\", \"type\": \"None\" },{ \"id\": \"5002\", \"type\": \"Glazed\" }]}";
    System.out.println(pretty(json));
}       

public static String pretty(String s) {
    StringBuilder sb = new StringBuilder();
    int indent = 0;
    char pre = 0;
    for (char c : s.toCharArray()) {
        if (Character.isWhitespace(c)) continue;
        if (c == ']' || c == '}') indent--;
        if (pre == '[' || pre == '{' || pre == ',' || c == ']' || c == '}') {
            sb.append('\n');
            for (int i = 0; i < indent; i++) sb.append("  ");
        }       
        sb.append(c);
        if (c == '[' || c == '{') indent++;
        pre = c;
    }       
    return sb.toString();
}
```

```python
def pretty_print(json):
    if not json:
        return ''
    
    result = []
    multiplier = 0
    i = 0
    
    while i < len(json):
        if json[i] in ['{', '[']:
            result.append('  ' * multiplier + json[i])
            multiplier += 1
            i += 1
        elif json[i] in ['}', ']']:
            multiplier -= 1
            result.append('  ' * multiplier + json[i])
            i += 1
        elif json[i] == ',':
            result[-1]+= ','
            i += 1
        else:
            start = i
            while i < len(json) and json[i] not in ['{', '}', ',', '[', ']']:
                i += 1
            curr_s = json[start:i]
            result.append('  ' * multiplier + curr_s)
    
    for r in result:
        print(r)
```

