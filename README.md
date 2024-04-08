# Chord-transposition
Transposition of chords for songs with lyrics and chords in plain text.
JavaScript

I have started to study a little bit of accompanying guitar, just as an amateur, not to participate in a musical group, just to be able to accompany with chords some songs that I like.

As a beginner, I read the song with the chords placed above the corresponding syllable. I have noticed that when going up or down in pitch, because for my voice the song is at a very high or very low all, as is logical, I have to change all the chords, i.e. transpose or transpose the chords.

Doing this on the fly, while reading the song, is quite complex for a beginner, so I have taken advantage of some JavaScript knowledge to try to make a web page with HTLM, CSS and JavaScript, which, locally, helps me to raise and lower the pitch of all the chords of the song.

The biggest problem I have encountered is that each transposed chord is on the same syllable as the original. For example, if over the word the syllable "ra" of the word "pirate" is the chord 'G' (in Anglo-Saxon notation) or 'SOL' (in Latin notation), by raising half a tone, it will be 'G#' or 'SOL#', and the 'G' or 'S' of 'sol' should appear over the 'r' of 'ra', and so for all the following chords of that line of the song lyrics, but by raising one character, it shifts the following chords to the right, and all the others. This may seem like a minor problem, but if you try to do it on your own, you'll see that it's not that simple.

As for the extensions or alterations of the chords, such as sevenths, ninths, augmented or diminished, etc., I have not had many problems, since it is a matter of cutting and pasting the same extensions to the transposed note.

Although I know it is not good practice, I have put in a single HTLM file the CSS styles and the JavaScript code, including the help text, so that any user who wants it, can download everything in a single file and run it locally. The code is made in such a way that it works locally, on your PC, without having to upload anything to a server.

Also the songs with the chords will be in the same folder as this HTLM file, or the one we indicate by making a small modification in the code.

The web page works as follows:

1) Select a song with the [View File] button.

2) The file containing a song with its chords must meet these conditions:
 
→ It has to be a plain text file (a TXT edited with notepad or an equivalent tool).

→ The song title will go in the file name in the form: SongName.txt (it cannot go inside the text file).

→ The first line and all the following odd lines will be for putting the chords, and in the even lines will be the lyrics of the song.

→ Between each chord there must be at least two spaces to facilitate the transposition; if these spaces cannot be inserted, the lyrics must be separated from the song, either by separating the words or the syllables (e.g.: se--pa---rado).

→ To see well the position of the chords above the corresponding syllables of the song lyrics, they should be written with a monospace font (each letter occupies one space, such as courier, liberation mono, etc.).

→ If a line of lyrics does not have chords, the top line corresponding to the chords must be left blank (line break), as happens for example, when putting "Chorus" to refer to the chorus of the song, or simply when that line of text drags the chord of the previous one and it is necessary to put it (odd lines always for chords and even lines for lyrics).

3) To increase the chords by half a tone, click on the [Increase] button and to decrease them by half a tone, click on the [Decrease] button.

4) You can edit the song and save it with the name you want by clicking on the [Save] button.

5) To clear the screen and load another song, click on the [Clear] button.

6) To change from traditional to modern chord format and vice versa there are the buttons [ T -> M ] and [ M -> T].

7) Example song:

FA7
Olha que coisa mais linda, 

              SOL7(13)
mais cheia de graça

(line without chords)

É ela, menina, 

              sol7
que vem e que passa

            DO7(9-)
Num doce balanço 

             la7(11)
a caminho do mar ...

-----------

The code of this web page is as follows:

```

<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name='Author' content='(c) Maria del Mar Martinez Herrera'/>
<title>Transponer Acordse de Canciones</title>

<style>
  /* Estilo para el área de edición y los botones */
  textarea {
    font-family: 'Courier', monospace;
    font-weight: bold;
    white-space: pre-wrap;
    width: 100%;
    height: 80%;
    margin-bottom: 5px;
  }
  .button {
  padding: 10px 20px; /* Ajusta el relleno */
  background-color: #007bff; /* Color de fondo */
  color: white; /* Color del texto */
  border: none; /* Elimina el borde */
  border-radius: 5px; /* Bordes redondeados */
  cursor: pointer; /* Cambia el cursor a una mano */
  font-size: 16px; /* Tamaño del texto */
  margin: 5px; /* Espaciado alrededor de los botones */
  }

  #fileNameDisplay {
    /* Estilos para el texto del nombre del archivo */
    font-family: 'Arial';
    margin-left: 10px;
  }
</style>

</head>

<body>

<input type="file" id="fileInput" style=display:none;" />
<button class="button" id="fileInputButton">Ver Archivo</button>
<span id="fileNameDisplay"></span><br>

<textarea id="content" rows="50" cols="90"></textarea><br>
<button class="button" id="aumentar"> [+] </button>
<button class="button" id="disminuir"> [-] </button>
<button class="button" id="cambioMT"> M -> T </button>
<button class="button" id="cambioTM"> T -> M </button>
<button class="button" id="save">Guardar</button>
<button class="button" id="salir">Limpiar</button>
<button class="button" id="ayuda">Ayuda</button>

<script>

    window.cifradoActual = ''; // variable global para cifrado activo
    window.esAyuda = 0; // variable para saber si estamos viendo la Ayuda

document.getElementById('fileInputButton').addEventListener('click', function() {
  document.getElementById('fileInput').click(); // Simula un click en el input de archivo real
  esAyuda = 0;
});

document.getElementById('fileInput').addEventListener('change', function(event) {
  var file = event.target.files[0]; // Obtiene el archivo seleccionado

  if (file) {
    var reader = new FileReader(); // Crea una instancia de FileReader para leer el archivo

    // Este evento se dispara después de que el contenido del archivo ha sido leído
    reader.onload = function(e) {
      var content = e.target.result; // Obtiene el contenido del archivo
      document.getElementById('content').value = content; // Inserta el contenido en el textarea
    };

    reader.readAsText(file, 'UTF-8'); // Lee el contenido del archivo como texto

    // Actualiza el texto del <span> con el nombre del archivo
    document.getElementById('fileNameDisplay').textContent = file.name;
  }

  cifradoActual = '';
  esAyuda = 0;
 
});


// -------------------------------------------------------
const acordesBaseT = ['dob','do#','do','reb','re#','re','mib','mi#','mi','fab','fa#','fa','solb','sol#','sol','lab','la#','la','sib','si#','si','DOb','DO#','DO','REb','RE#','RE','MIb','MI#','MI','FAb','FA#','FA','SOLb','SOL#','SOL','LAb','LA#','LA','SIb','SI#','SI','Dob','Do#','Do','Reb','Re#','Re','Mib','Mi#','Mi','Fab','Fa#','Fa','Solb','Sol#','Sol','Lab','La#','La','Sib','Si#','Si']

const acordesAumenT = ['do','re','do#','re','mi','re#','mi','fa#','fa','fa','sol','fa#','sol','la','sol#','la','si','la#','si','do#','do','DO','RE','DO#','RE','MI','RE#','MI','FA#','FA','FA','SOL','FA#','SOL','LA','SOL#','LA','SI','LA#','SI','DO#','DO','Do','Re','Do#','Re','Mi','Re#','Mi','Fa#','Fa','Fa','Sol','Fa#','Sol','La','Sol#','La','Si','La#','Si','Do#','Do']

const acordesDismiT = ['sib','do','si','do','re','do#','re','mi','re#','mib','fa','mi','fa','sol','fa#','sol','la','lab','la','si','la#','SIb','DO','SI','DO','RE','DO#','RE','MI','RE#','MIb','FA','MI','FA','SOL','FA#','SOL','LA','LAb','LA','SI','LA#','Sib','Do','Si','Do','Re','Do#','Re','Mi','Re#','Mib','Fa','Mi','Fa','Sol','Fa#','Sol','La','Lab','La','Si','La#']

const acordesBaseM = ['Cbm','C#m','Cm','Dbm','D#m','Dm','Ebm','E#m','Em','Fbm','F#m','Fm','Gbm','G#m','Gm','Abm','A#m','Am','Bbm','B#m','Bm','Cb','C#','C','Db','D#','D','Eb','E#','E','Fb','F#','F','Gb','G#','G','Ab','A#','A','Bb','B#','B','Cb','C#','C','Db','D#','D','Eb','E#','E','Fb','F#','F','Gb','G#','G','Ab','A#','A','Bb','B#','B']

const acordesAumenM = ['Cm','Dm','C#m','Dm','Em','D#m','Em','F#m','Fm','Fm','Gm','F#m','Gm','Am','G#m','Am','Bm','A#m','Bm','C#m','Cm','Cm','D','C#','D','E','D#','E','F#','F','F','G','F#','G','A','G#','A','B','A#','B','C#','C','C','D','C#','D','E','D#','E','F#','F','F','G','F#','G','A','G#','A','B','A#','B','C#','C']

const acordesDismiM = ['Bbm','Cm','Bm','Cm','Dm','C#m','Dm','Em','D#m','Ebm','Fm','Em','Fm','Gm','F#m','Gm','Am','Abm','Am','Bm','A#m','Bb','C','B','C','D','C#','D','E','D#','Eb','F','E','F','G','F#','G','A','Ab','A','B','A#','Bb','C','B','C','D','C#','D','E','D#','Eb','F','E','F','G','F#','G','A','Ab','A','B','A#']

// -------------------------------------------------------
// Ver tipo de cifrado y si es ayuda
function verificarCifrado() {
    const areaTexto = document.getElementById('content');
    const lineas = areaTexto.value.split('\n');
    if (lineas[0].includes("= AYUDA =")) {
       esAyuda = 1;
    } else {
       esAyuda = 0;
    }
    for (let i = 0; i < lineas.length; i += 2) {
        const linea = lineas[i];
        const acordeTradicional= acordesBaseT.some(acorde => linea.includes(acorde));
        if (acordeTradicional) {
            return ("T");
        } else {
        }
    }
    return ("M");
}
// -------------------------------------------------------

// Transponer acorde
function transportarAcorde(acorde, ad) { // acorde = trozo línea con acorde, ad = 1 aumentar -1 disminuir tono, 0 cambiar cifrado

   let nuevoAcorde = '';
   let diferencia = 0;
   let resto = '';
   let lenAcorde = acorde.length;

 if (ad === 0) {

   let lenT = 0;
   let lenM = 0;

   if (cifradoActual === "T") { // estoy cambiando de Tradicioal a Moderno
      // Encuentra el índice del primer acorde que está incluido en la cadena
      const indice = acordesBaseT.findIndex(acor => acorde.includes(acor));
      if (indice === -1) { return acorde; }
      lenT = acordesBaseT[indice].length;
      lenM = acordesBaseM[indice].length;
      diferencia = lenT - lenM;
      resto = acorde.slice(-(lenAcorde-lenT));
      let nuevoAcorde = '';
      nuevoAcorde = acordesBaseM[indice] + resto;

      // de moderno a tradicional la variable diferencia siempre va a ser igual o menor cero
      if (diferencia !== 0) {
         nuevoAcorde = nuevoAcorde + ' '.repeat(diferencia); //añado espacios
      }

      return nuevoAcorde;
   }

   if (cifradoActual === "M") { // estoy cambiando de Moderno a Tradicional
      // Encuentra el índice del primer acorde que está incluido en la cadena
      const indice = acordesBaseM.findIndex(acor => acorde.includes(acor));
      if (indice === -1) { return acorde; }
      lenT = acordesBaseT[indice].length;
      lenM = acordesBaseM[indice].length;
      diferencia = lenT - lenM;
      resto = acorde.slice(-(lenAcorde-lenM));
      let nuevoAcorde = '';
      nuevoAcorde = acordesBaseT[indice] + resto;

      // de moderno a tradicional la variable diferencia siempre va a ser igual o menor cero
      if (diferencia !== 0) {
         nuevoAcorde = nuevoAcorde.slice(0, -diferencia);
         //nuevoAcorde = nuevoAcorde.susbstr(0, nuevoAcorde.length - diferencia); //quito espacios
      }

      return nuevoAcorde;
   }

 } else {

   if (cifradoActual === "T") {
      let indice = acordesBaseT.findIndex(acor => acorde.includes(acor));
      if (indice === -1) { return acorde; }
      let lenAB = acordesBaseT[indice].length;
      let lenAA = acordesAumenT[indice].length;
      let lenAD = acordesDismiT[indice].length;
      resto = acorde.slice(-(lenAcorde-lenAB));
      if (ad > 0) { // si es 1 Aumentar medio tono
          diferencia = lenAB - lenAA;   
          nuevoAcorde = acordesAumenT[indice] + resto;
      } else {  // si es -1 Disminuir medio tono (si es 0 ya return antes)
          diferencia = lenAB - lenAD; 
          nuevoAcorde = acordesDismiT[indice] + resto;
      }  
 
      if (diferencia > 0) { // si la diferencia es positiva
          nuevoAcorde = nuevoAcorde + ' '.repeat(diferencia);
      }
      if (diferencia < 0) {  // si es negativa
          nuevoAcorde = nuevoAcorde.slice(0, diferencia);
      }
      return nuevoAcorde; // sale con return
   } // fin del if cifradoActual T

   if (cifradoActual === "M") {
      let indice = acordesBaseM.findIndex(acor => acorde.includes(acor));
      if (indice === -1) { return acorde; }
      let lenAB = acordesBaseM[indice].length;
      let lenAA = acordesAumenM[indice].length;
      let lenAD = acordesDismiM[indice].length;
      resto = acorde.slice(-(lenAcorde-lenAB));
      if (ad > 0) { // si es 1 Aumentar medio tono
          diferencia = lenAB - lenAA;   
          nuevoAcorde = acordesAumenM[indice] + resto;
      } else {  // si es -1 Disminuir medio tono (si es 0 ya return antes)
          diferencia = lenAB - lenAD; 
          nuevoAcorde = acordesDismiM[indice] + resto;
      }   

      if (diferencia > 0) { // si la diferencia es positiva
          nuevoAcorde = nuevoAcorde + ' '.repeat(diferencia);
      }
      if (diferencia < 0) {  // si es negativa
          nuevoAcorde = nuevoAcorde.slice(0, diferencia);
      }
   } // fin del if cifradoActual M

 } // fin de else ad = 0

   return nuevoAcorde;
}



// Función para procesar las líneas de acordes
function lineasAcordes(lineaAcordes, ad) {
    let lineaAcordesTrans = ''; // variable donde irán las líneas modificadas (transpuestas)
    lineaAcordes += '   '; // le añado tres espacios para facilitar el tratamiento

    // Miro si hay espacios al inicio de la línea, y si los hay los guardo en lineaAcordesTrans
    let espaciosInicio = lineaAcordes.match(/^\s*/)[0];
    lineaAcordesTrans += espaciosInicio;

    //Elimino de la izquierda de la línea lineaAcordes los espacios
    lineaAcordes = lineaAcordes.slice(espaciosInicio.length);
    lenLineaAcordes = lineaAcordes.length;


   // Bucle para procesar cada acorde en la línea
    while (lenLineaAcordes > 0) {
    
       //Toma los primeros caracteres no espacio más los espacios siguientes hasta que encuentra caracteres no espacio
       let acorde = lineaAcordes.match(/^\S+\s*/)[0];

       //Elimino ese trozo de lineaAcordes
       lineaAcordes = lineaAcordes.slice(acorde.length);
       lenLineaAcordes = lineaAcordes.length;
    
       //Envio trozo con acorde y lo devuelvo transpuesto
       lineaAcordesTrans += transportarAcorde(acorde, ad);
    
    }

    lineaAcordesTrans = lineaAcordesTrans.trimEnd(); // le quito los espacios del final
    return lineaAcordesTrans;
}


// Función principal para procesar el contenido del textarea
function procesarCancion(ad) {
    // Obtener el contenido del textarea
    let contenido = document.getElementById('content').value;
    // Dividir el contenido en líneas
    let lineas = contenido.split('\n');
    
    // Variable para almacenar la canción transformada
    let cancionTrans = '';

    // Iterar sobre las líneas
    for (let i = 0; i < lineas.length; i++) {
        // Para las líneas pares (acordes), procesarlas y añadirlas a cancionTrans
        if (i % 2 === 0) { // Recordando que las líneas pares en programación comienzan con índice 0
            let lineaProcesada = lineasAcordes(lineas[i], ad);
            cancionTrans += lineaProcesada + '\n';
        } else {
            // Para las líneas impares (letra), solo añadirlas a cancionTrans
            cancionTrans += lineas[i] + '\n';
        }
    }

    // Cargo la canción transpuesta en el área de texto
    cancionTrans = cancionTrans.replace(/\n+$/, ''); // quita los INTRO de final de archivo
    document.getElementById('content').value = cancionTrans;

}


document.getElementById('disminuir').addEventListener('click', function() {
    cifradoActual = verificarCifrado();
    if (esAyuda === 0) {
       procesarCancion(-1);
    }
});

document.getElementById('aumentar').addEventListener('click', function() {
    cifradoActual = verificarCifrado();
    if (esAyuda === 0) {
       procesarCancion(1);
    }
});


document.getElementById('cambioMT').addEventListener('click', function() {
    cifradoActual = verificarCifrado();
    if (esAyuda === 0) {
       if (cifradoActual === "T") {
       } else {
          cifradoActual = "M";
          procesarCancion(0); // 0 es cuando se procesa con cambio de cifrado de M a T o de T a M
          cifradoActual = "T";
       }
    }
});


document.getElementById('cambioTM').addEventListener('click', function() {
    cifradoActual = verificarCifrado();
    if (esAyuda === 0) { 
       if (cifradoActual === "M") { 
       } else {
          cifradoActual = "T";
          procesarCancion(0); // 0 es cuando se procesa con cambio de cifrado de M a T o de T a M
          cifradoActual = "M";
       }
    }
});


document.getElementById('save').addEventListener('click', function() {
    const text = document.getElementById('content').value;
    if (!text.trim()) {
        console.log('No hay texto seleccionado para guardar');
        return;
    }
    // Si hay texto continúa
    const blob = new Blob([text], { type: 'text/plain' });

    // Obtiene el nombre del archivo del elemento <span>
    const fileName = document.getElementById('fileNameDisplay').textContent || 'fileInput.txt';

    const anchor = document.createElement('a');
    anchor.download = fileName; // Usa el nombre recuperado para el atributo download
    anchor.href = window.URL.createObjectURL(blob);
    anchor.click();
    window.URL.revokeObjectURL(anchor.href);
});


document.getElementById('salir').addEventListener('click', function() {
    // Limpiar el contenido del área de texto
    document.getElementById('content').value = '';
    // Limpiar el nombre del archivo mostrado
    document.getElementById('fileNameDisplay').textContent = '';
    // Intentar enfocar el input de tipo file
    const fileInput = document.getElementById('fileInput');
    fileInput.value = ''; // Esto asegura que el input esté limpio y listo para una nueva selección
    fileInput.focus();
    esAyuda = 0; // Si limpiamos el area de texto ya no está la ayuda en pantalla.
});

document.getElementById('ayuda').addEventListener('click', function() {
    const textoAyuda = '============= AYUDA ============\n\n1) Selecciona una canción con el botón [Ver Archivo]\n\n2) El archivo que contiene una canción con sus acordes deberá cumplir estas condiciones:\n→ Ha de ser un archivo de texto plano (un TXT editado con el bloc de notas o una herramienta equivalente)\n→ El título de canción irá en el nombre del archivo de la forma: NombreCancion.txt (no puede ir dentro del archivo de texto)\n→ La primera línea y todas las impares siguientes serán para poner los acordes, y en las líneas pares irá la letra de la canción.\n→ Entre cada acorde tiene que haber, al menos, dos espacios para facilitar la transposición; si no se pueden intercalar esos espacios habrá que separar la letra de la canción, bien separando las palabras o las sílabas (ejemplo: se--pa---rado).\n→ Para ver bien la posición de los acordes encima de las sílabas correspondientes de la letra de la canción, se deberá escribir con un tipo de letra monoespacio (cada letra ocupa un espacio, como courier, liberation mono, etc.)\n→ Si una línea de letra de canción no tiene acordes, habrá que dejar en blanco (salto de línea) la línea superior que corresponde a los acordes, como sucede por ejemplo, al poner Estribillo para hacer referencia al estribillo de la canción, o simplemente cuando esa línea de texto arrastre el acorde de la anterior y sea necesario ponerla (líneas impares siempre para acordes y pares para letra).\n\n3) Para aumentar medio tono los acordes pulsaremos sobre el botón [Aumentar] y para disminuir medio tono sobre el botón [Disminuir].\n\n4) Podemos editar la canción y guardarla con el nombre que queramos, para ello pulsaremos sobre el botón [Guardar].\n\n5) Para limpiar la pantalla y cargar otra canción, pulsaremos el botón [Limpiar].\n\n6) Para cambiar de formato de acorde tradicional a moderno y viceversa están los botones [ T -> M ] y [ M -> T]\n\n7) Ejemplo de canción:\nFA7\nMira qué cosa más linda,\n             SOL7(13)\nmás llena de gracia\n\nes esa muchacha,\n                sol7\nque viene y que pasa\n            DO7(9-)\ncon su balanceo,\n           la7(11)\ncamino del mar.\n';
    const textarea = document.getElementById('content');
    textarea.value = textoAyuda;
    document.getElementById('fileNameDisplay').textContent = "AYUDA";
    //textarea.readOnly = true;
    esAyuda = 1;
});

</script>

</body>
</html>

```

I am sure, that this code can be improved, reusing or reorganizing some functions into smaller functions, but, for now, this beta version works.

Any advice or input would be appreciated.

