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
Запрос xQuery с поиском множества значений поля
```js
var tests_id = [id1, id2];
var end_tests = XQuery("for $elem in test_learnings where MatchSome($elem/assessment_id,("+tests_id.join(",")+")) return $elem")
```
Выборка подчиненных для функцонального руководителя
```js
//boss_type_id - id типа функц. руководителья
var users = ArraySelectDistinct( tools.get_all_sub_persons_by_func_manager_id(hr_id, "collaborator,subdivision,org,position", boss_type_id), "id")
```
Выборка сотрудников заданного подразделения
```js
var users = tools.get_sub_person_ids_by_subdivision_id(sub_id);
```
#Манипуляции с объектами (карточками)
Получение значения настраиваемого поля
```js
var collDoc = OpenDoc(UrlFromDocID(coll_id));
var value = collDoc.TopElem.custom_elems.ObtainChildByKey("value").value;
```
Копирование данных из одного элемента в другой, включая дочерние элементы. Удобный способ создавать объекты по эталону.
```js
elemDoc.TopElem.AssignElem(otherElemDoc);
```
А так удобно заполнять дочерние элементы:
```js
//находим нужный дочерний элемент
fldTutor = docEvent.TopElem.tutors.ObtainChildByKey( id_сотрудника );
//fldTutor = docEvent.TopElem.collaborators.AddChild();
//заполняем этот элемент из карточки сотрудника
tools.common_filling( 'collaborator', fldTutor, id_сотрудника );
docEvent.Save();
```
#Присвоения отрицательного значения
```js
//нужно присвоить переменной -1, в WT это можно сделать так
var num = 0-5;
//вывод
alert(StrSignedInt(num));
```
#Работа с файлами
Локальный url выглядит так `x-local://wt/web/dir`, wt - директория с файлами сайта.

Чтобы получить абсолютный путь используем метод `UrlToFilePath`
```js

```
#Работа с Url
Формирование Url
```js
link = UrlAppendPath( global_settings.settings.portal_base_url, '/view_doc.html?mode=career_reserve&object_id='+objDocSecID+'&doc_id='+doc_id );
```
