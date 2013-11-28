#Часто используемые конструкции
##Регулярные выражения
```js
var objRegExp = tools_web.reg_exp_init();
//var objRegExp = new ActiveXObject('VBScript.RegExp');
objRegExp.Global = true;
objRegExp.IgnoreCase = true;
objRegExp.MultiLine = true;

//определяем паттерн
objRegExp.Pattern = "\\n";

//текст для разбора
var text = "dolor ipsum"
```
Поиск фрагментов строки, соответствующих заданному выражению
```js
//метод Test возвращает true, если фрагмент, соответствующий выражению, 
//в заданной строке найден, и false в противном случае
objRegExp.Test(text);
```
```js
//Execute возвращает объект - последовательность фрагментов текста, совпавших с шаблоном
var mathes = objRegExp.Execute(text);

try{
  var mathes = objRegExp.Execute(text);	    
  return mathes.item(0).SubMatches(0);
}catch(exp){
  return "";
}
```
```js
//замена найденного
text = objRegExp.Replace(text, "fragment");
```
##Запросы к БД
Пример sql запроса с объедиением нескольких таблиц, выборкой настраиваемого поля
```js
var data = XQuery("
	SELECT
	c.id as id,
	cs.fullname as fullname,
	c.data.value('collaborator/custom_elems/custom_elem[name=\"GK_hire_date\"][1]/value', 'varchar(512)') as GK_hire_date,
	c.data.value('collaborator/org_name', 'varchar(512)') as org_name,
	c.data.value('collaborator/is_dismiss', 'bit') as is_dismiss

	FROM collaborator as c 
	INNER JOIN collaborators as cs ON c.id=cs.id 
	INNER JOIN Positions as ps ON cs.position_id=ps.id
");
```
Запрос xQuery с фильтром по настраиваемому полю
```js
var data = XQuery("for $elem in collaborators where  doc-contains($elem/id,'wt_data','[collab_type=руководитель~string]') return $elem")
```
#Манипуляции с объектами (карточками)
Получение значения настраиваемого поля
```js
var collDoc = OpenDoc(UrlFromDocID(coll_id));
var value = collDoc.TopElem.custom_elems.ObtainChildByKey("value").value;
```
