kopiere die rundum-dateien aus dem boilerplate directory.

```
cp -r wf/boilerplate wf/my_wf
```

benenne boilerplate.bpmn in my_wf.bpmn um:

```
cd wf/my_wf
mv boilerplate.bpmn my_wf.bpmn
```

in my_wf.bpmn string-replace 'boilerplate_v0.1' zu 'my_wf_v0.1'.

in STS, benenne die 'hello_boilerplate' activity in 'hello_wf' um. dieser
aktivity name wird in forms.xml referenziert.

rechts-clicke auf die activity, dann 'show property view'. im tab 'user
task' benenne task name, comment und group_id zu 'hello_wf' um.
behalte die #{} um group id.  group id wird in actors.xml
referenziert.

jede activity muss in der local variable list vorkommen.  clicke auf
den canvas, um auf die general properties zu kommen, da auf 'data
items'.  in der local variable list benenne 'hello_boilerplate' zu
'hello_wf' um.

in actors.xml werden die berechtigungen gesetzt. referenziere deine
'hello_wf' activity:

```
<actor task="hello_wf" type="GROUP" defaultvalue="Administratoren,NUM-Study Nurse,NUM-StudyNurseMTLA-Kombi"/>
```

in forms.xml werden die java klassen gesetzt. referenziere die
'hello_wf' activity:

```
<form task="hello_wf" form="my_wf.HelloWF"/>
```

my_wf.HelloWF ist der name der java klasse fuer die hello_wf activity.
lege sie an:

```
cp -r src/main/java/boilerplate src/main/java/my_wf
mv src/main/java/my_wf/HelloBoilerplate.java
src/main/java/my_wf/HelloWF.java
```

in HelloWF.java aendere package und den namen der klasse: 

```
package my_wf;
...
public class HelloWF extends WorkflowTaskDialogContent {
...
private static final Logger LOG = LoggerFactory.getLogger(HelloWF.class);
```

die getViewContent() in HelloWF methode baut das ui. aendere den
string den helloLabel anzeigt zu "hello my workflow".

```
helloLabel.setValue("hello my workflow");
```

im verzeichnis assembly/ kopiere boilerplate.xml zu my_wf.xml.

```
cd assembly/
cp boilerplate.xml my_wf.xml
```

in my_wf.xml aendere die id zu my_wf:

```
<id>my_wf</id>
```

und setze das verzeichnis aus dem die rundum-dateien geladen werden:

```
<directory>wf/my_wf</directory>
```

in pom.xml (im project root) melde my_wf.xml als build profile an.
fuege es als descriptor zu einem profile (z.b. dev) hinzu:

```
<descriptor>src/assembly/my_wf.xml</descriptor>
```

baue das profil, hier z.b. dev:

```
mvn clean install -P dev
```

jetzt muesste eine datei \*my_wf.zip im target/ folder liegen.

ins cxx laden unter workflow > workflow-prozesse > plus button, zip
auswaehlen, speichern.

starten mit play button, dann zu workflow > workflow-aufgaben gehen,
in 'gruppe inbox' my_wf auswaehlen, mit play button starten.

## was muss man beachten wenn man einen neuen workflow aufsetzt

- wenn das parsen von CatalogData einen null pointer gibt, dann
vielleicht eine catalog.xml reinladen wo
`<CatalogueData></CatalogueData>` leer ist.

- jede task (box im pbmn) braucht eine variable die so heisst wie sie in
der 'Local Variable List for Process' heisst (clicken auf den canvas, dann
property, dann data items).

## welche methoden ueberschreibt eine workflow klasse?

es muss nur getViewContent() ueberschrieben werden. 

saveTemporary() wird zum zwischenspeichern ueberschrieben.  save() wird zum endgueltig speichern ueberschrieben (beim click auf 'naechste aktivitaet').