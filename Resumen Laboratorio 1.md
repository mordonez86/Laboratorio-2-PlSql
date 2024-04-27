# Primer Parcial Laboratorio 1 Programacion Orientada a Objetos con Java

```Java
//Proceso de instanciado:
Auto miAuto = new Auto()
// se da en 3 partes
// Primero se crea la variable
Auto miAuto // miAuto es uana variable q no apunta a nada
//luego se crea el objeto
new Auto() // se crea sin un valor por defecto
// con el igual guardo el objeto en la variable
Auto miAuto = new Auto() // se unene las dos partes mediante el igual

//Hasta aca tenemos el objeto en blanco, podemos asignarle valores mediante los setters
miauto.setMarca("ford")
```

### Encapsulamiento

* ##### La unidad mas elemental de encapsulamiento es la ***clase***
* ##### Si un elemento es privado implica que su visibilidad  es solo para si mismo, pero sobre todo lo mas importante es evitar que otros objetos no accedan o cambien sus valores libremente, como mucho mediante un set, se podra crear un metodo para alterar estos valores, pero controlo en el objeto la forma de modificar sus valores de los datos.
* ##### Los atributos arrancan privados, y luego se tienen operaciones(si se necesita) para alterar los valores de sus datos

##### Metodo Main:
Siempre debe haber una clase principal (por convencion llamada main) con un metodo publico que sea RUN , para usar como punto de entrada al progama

Cuando asignamos valores a las instancias de los objetos, igual que en javascript o python podemos utilizar el this.atributo, para hacer referencia al atributo de ESA instancia y no causar ambiguedades en el codigo cuando asignamos el valor:
```Java
Auto{
    Private String marca;
    Private String modelo;

    setMarca(String marca){
        this.marca=marca; // aca hacemos uso del this para evitar ambiguedades.
    };
}
```
#### Constructores:

Utilizamos constructores cuando queremos asignar un valor por defecto a un atributo de una instancia, no solo para asignarle un valor por defecto, sino porque puede ser un valor que yo NO QUIERO que sea alterado por lo tanto no le genero un setAtributo().
Los constructores son metodos publicos que no tienen un valor de retorno.

```Java
// Ejemplo de constructor:
public Class Auto(){
    private String marca;
    private String modelo;
    private int anioFabricacion;
    //constructor
    public Auto(String marca){
        this.marca=marca;
    }
}
// Ejemplo de instanciado:
Auto miauto = new Auto("Ford");



// Tambien se pueden sobrecargar constructores con diferentes tipos y cantidades de argumentos.
public Class Auto(){
    private String marca;
    private String modelo;
    private int anioFabricacion;
    //constructor
    public Auto(String marca){
        this.marca=marca;
    }
    // constructor sobecargado, aca podemos inicializar un auto sin pasarle la marca
     public Auto(){        
    }
}
```
Para hacer uso de la sobrecarga de constructores, lo que importa no es el nombre de los argumentos, sino el orden y tipo de sus argumentos
```Java
public Class Auto(){
    private String marca;
    private String modelo;
    private int anioFabricacion;

    //constructor
    public Auto(String marca){
        this.marca=marca;
    }
    // constructor sobecargado, ESTO DA ERROR PORQUE YA TENEMOS UN CONSTRUCTOR QUE ESPERA UN STRING, por mas que su nombre sea distinto.
    public Auto(String modelo){
        this.modelo=modelo;
    }
}
```




#### Para sobrecargar los constructores , yo debo alterar , el tipo ,cantidad y orden de los parametros para evitar su duplicidad o ambiguedad
Esto significa que puedo llamar desde adentro de un constructor, otro constructor para no duplicar codigo, pero solo puedo hacerlo 1 vez, porque uso UN solo constructor para construir UNA instancia en particular, este comportamiento se obtiene usando this(parametros), ***atencion porque no es el this.marca del ejemplo anterior***

---
#### Clase del 22

>***Orientacion a objetos, puede ser que pierda un poco de popularidad con respecto a la progamacion funcional, por una custion de optimizar la velocidad de cara al usuario. Pero donde se necesita realmente robustez,mantenimiento,estructura y formalidad la programacion orientada a objetos es claramente superior***


##### Sobrecarga de Metodos

**Motivo: Mantenimiento
Objetivo: Reutilizar codigo**

Muchas veces necesitamos hacer la misma operacion, pero modificando algunos parametros por ejemplo si se tiene un metodo devolver lista y de ese metodo dependen otras cosas y ahora quiero obtener una lista pero ordenada de ota manera, no podemos afectar a ese metodo directamente, seguramente el primer intento seria crear un nuevo metodo y por parametros cambiar su comportamiento, pero esto empieza a complejizar y repetir codigo, lo que empieza a dejar de ser cohesivo y empieza a volverse dificil de mantener o actualizar.


> ***Algunos lenguajes conocen esto como POLIMORFISMO ESTATICO , es decir en tiempo de compilacion modificamos el funcionamiento de las funciones (aclaro que es funciones y no metodos porque esto en orientacion orientada a objetos se utiliza el concepto de Herencia)***

### Los objetos se comunican entre si mediante mensajes , los mensajes son ... con una carga paga ....  volver a ver esta definicion en el min 03:18:00

---
#### Metodos estaticos, concepto de "static":

Se usa para valores que queremos que cambien sin importar de las instancias que lo utilicen o tambien se puede decir como quitarle el poder a las instancias de alterarle el valor a algo
estos atributos estaticos tambien son llamados ATRIBUTOS DE CLASE, lo que significa que si tengo un atributo dentro de una clase, todas sus instancias van a tener asignado ese valor que se asigno quiza en la primer instanciacion.
En este caso podemos hacer un determinado metodo sobre una CLASE base , lo que significa que todas sus instancias van a tener ese valor, lo que tenemos que tener en cuenta es que el **this** deja de tener sentido porque no va a trabajar sobre cada instancia sino sobre TODAS

>static no es para hacer valores **CONSTANTES**, sino para hacer valores **INVARIANTES**
Para hacer valores constantes tenemos que hacer

```Java
public static final int TEMPERATURA =110 // final nos da la idea de constante y se hace con mayuscula y guiones bajos
```
### Herencia
En los objetos ,podemos hallar un orden de importancia entre si (no importancia desde un lugar propiamente dicho, sino desde el lugar de entender de cuan abarcativo puede ser su funcionamiento)ej:

un auto es un auto, puede ser deportivo o no , puede ser tipo camioneta y tener un espacio para carga, pero todos son AUTOS, al mismo tiempo los autos pueden ser englobados dentro del objeto VEHICULO, la caracteristica del vehiculo es transportar personas, y asi ir yendo a lo mas general hacia arriba o haciendonos mas especificos nos vamos alejando del ***MODELO IDEAL***, ideal desde un lugar de idealizacion de lo que un VEHICULO DEBE SER que es ALGO PARA TRANSPORTAR PERSONAS, luego seran autos, camiones, camionetas, bicicletas, pero todos cumplen con el IDEAL o IDEA de vehiculo.

esto puede ser identificado como ***ORDEN SUPERIOR***, cuando se esta mas cerca del ***IDEAL*** del **MODELO MENTAL**, u ***ORDEN INFERIOR*** cuanto mas detallado se tiene el objeto

teniendo en cuenta todas estas formas de clasificar la realidad podemos ir agrupandolos por caracteristicas en comun con respecto a los demas

![alt text](image.png)

Como observamos en la imagen , vamos de un orden superior a un orden inferior mas detallado, pero entre si pueden tener caracteristicas que los unan o diferencien pudiendolos agrupar en un ***NIVEL***

llevandolo a clases , vemos que la relacion entre clases se da en vez de la idea de ***TIENE UN*** la idea de ***ES UN*** es decir un rottwailler ***ES UN*** perro esta idea de es un, nos acerca a la idea de que de las clases no nos importa que son o que atributos tienen , ya que preferentemente sus atributos van a ser privados a si mismo, sino empieza a importarnos ***QUE ES LO QUE HACEN*** las clases

por lo tanto un rottwailer ***ES UN*** perro , porque hace POR LO MENOS lo  que hace un perro(ladrar,correr,comer) , esto lo trasladamos al concepto de herencia porque en vez de agrupar las clases por sus caracteristicas , las agrupamos por sus METODOS entendiendo que es lo que pueden hacer

esta relacion se hace usando la palabra reservada **EXTENDS**

```Java
//aca vemos implementada la clase PERRO
public class Perro(){
    private String nombre= new String;
    private Integer edad= new Integer;

    String ladrar(){
        return "GUAU";
    }
}
//aca creamos la clase Rottwailler la cual HEREDA de la clase PERRO sus METODOS para no tener que duplicar codigo

//logica del rottwailer, aca si queremos crear especificamente la clase rottwailer, entonces esta 
//clase va a tener que hacer COMO MINIMO lo que hace un perro, pero cosas especificas del rottwailer que me 
//hace tener la necesidad de implementarla por separado.

public class Rottwailler extends Perro(){
    
}
```

De hecho en ese caso podemos instanciar un rottwailer y almacenar el rottwailer en una variable del tipo Perro
```Java
Perro r = new Rottwailler() // estamos guardando en una variable tipo PERRO un objeto tipo Rotwailler
```

>#### SOBRE-ESCRITURA DE METODOS ES TENER LA MISMA ESTRUCTURA, ES DECIR CON LAS MISMA CANTIDAD DE ATRIBUTOS,TIPO Y ORDEN en distintos metodos, a diferencia del polimorfismo statico de la sobrecarga de constructores donde ***NO PODIAMOS*** tener mismo orden cantidad y tipo de parametros


lo que hice mas arriba de crear la variable de tipo perro e instanciar un rottwailer me sirve para poder desde adentro de rottwailer llamar un metodo de tipo perro y adaptar o agregar el funcionamiento que le quiera dar el rottwailer, para que no sea ambigua la llamada a LADRAR de la clase PERRO , y no LADRAR DE LA CLASE HUSKY entonces en vez del modificador THIS ,  se usa el modificador SUPER.ladrar()

Hay que tener en cuenta que si utilizamos la variable mas abarcativa (PERRO) y la instancia HUSKY tiene un metodo exclusivo de el que no existe en la clase PERRO perdemos la posibilidad de acceder al metodo exclusivo de husky, ya que nos da un error en tiempo de compilacion, ya que va a buscar el metodoHusky en la clase Perro y no existe.

tener encuenta el comentario todos los huskys, son perros , pero no todos los perros son huskys por lo tanto, la herencia es algo que se da de un orden superior a un orden inferior.


## Polimorfismo
>Para que exista ***POLIMORFISMO , debe existir un CONTRATO*** sino lo que tenemos es un comportamiento polimorfico utilizando recargas,polimorfismo estatico, o clases abstractas

clase abstracta permite reutilizar codigo, aprovechando la herencia , ceder a las hijas la posibilidad de especificar como van a realizar sus metodos(tambien las OBLIGA ) lo que significa que no se puede instanciar esa clase abstracta sino las que sean de orden inferior, especificando su comportamiento.

las interfaces nos sirven para heredar comportamiento en clases a distintos niveles ,con el contrato en el medio
