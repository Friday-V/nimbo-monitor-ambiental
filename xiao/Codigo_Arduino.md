/*
  NIMBO - Wio Terminal  ·  FIRMWARE  ·  FASE 4.2 (mascota de escritorio: su dia)
  ======================================================================
  Nuevo en 4.2 (26-nov 16:09: "ahora es muy dificil que se enoje"):
    - Toques: 1 = te pone atencion ("si?"), 2 o 3 = se rie, 4 o mas = "YA!" (enojo).
  Nuevo en 4.1 (26-nov 15:47, prueba en la Wio):
    - Sin lagrimita.
    - Los botones de arriba (A = caricia) ya no cuentan como toques: mientras aprietas un
      boton y 0.6 s despues no se cuentan toques (antes las caricias acababan en "YA!").
    - Toques mas confiables (se ponia enojado/emocionado al azar con vibraciones de la
      mesa): un golpe cuenta una sola vez aunque rebote (250 ms), umbral un poco mas alto
      (0.47 / 0.34 / 0.25 g), y "YA!" con 8 toques (antes 6).
    - Acostado: mientras esta acostado no lo despiertan la voz ni los ruidos; solo
      levantarlo, tocarlo o un boton (en un salon con gente lo despertaba la voz).
    - Risa y cosquillas: se sacude mas lento (3 veces por segundo, antes 7) y mas suave.
    - El amarillo dura mas: el color cambia mas despacio entre caras (~3 s) y el susto,
      el mareo, el "YA!" y el estar chueco ya no cambian el color (solo el aire lo cambia).
  Nuevo en 4.0 (26-nov 15:33): NIMBO sigue tu dia (la hora la manda la compu: HORA:)
    - BUENOS DIAS (6 a 11 h): la primera vez que le haces caso en la manana se estira,
      sonrie y dice "Buenos dias!" (una vez al dia).
    - NOCHE (22 a 6 h): mas dormilon: se aburre a los 2 min y se duerme a los 5 (de dia
      8 y 20), parpados un poco mas pesados; la primera vez dice "Ya es de noche..." y bosteza.
    - CELEBRA CUANDO VENTILAS: si el CO2 baja rapido (ventana abierta) hace un bailecito,
      se rie, toca una melodia y dice "Aire fresco! Gracias".
    - RESUMEN DEL DIA: cuenta los minutos en cada estado y las veces que ventilaste. A las
      20 h lo muestra solo (una vez); en OJOS, joystick ARRIBA lo muestra cuando quieras.
      (Cuenta desde la medianoche mientras esta prendido.)
    - PAUSA: la compu sabe si la estas usando (teclado/raton). Si llevas 50 min seguidos,
      nimbo_ia.py manda GESTO:estirar + AVISO: y NIMBO se estira y te sugiere pararte.
    - Comandos USB nuevos: HORA:<unix>,<zona_s>  AVISO:<l1|l2>  GESTO:estirar
  Nuevo en 3.9 (26-nov 15:25):
    - El color de los ojos = como esta el aire. En las reacciones (YA!, chueco) los ojos
      se quedan de su color (amarillo/lima si el aire esta bien); solo cambia la forma.
    - BOQUITA en las interacciones: sonrisa que se mueve (caricias, cosquillas, saludo),
      "o" chiquita que pregunta (1-2 toques), dientes apretados coral (YA!), "O" (susto),
      boca ondulada (mareado), boca que se abre grande (bostezo).
    - LAGRIMITA: un "cachito" turquesa en la parte de abajo del ojo, del lado de la nariz,
      unos segundos: despues de bostezar y de vez en cuando si el aire esta Preocupado/Alerta.
    - ACOSTADO: si lo acuestas boca arriba (pantalla al techo) bosteza y se duerme; al
      levantarlo despierta y te saluda. Acostado ya no cuenta como "chueco".
    - ABURRIDO: 8 min sin hacerle caso -> parpados pesados, zzz flotando y bostezos de vez
      en cuando; a los 20 min se duerme (dormido tambien muestra zzz).
    - HISTORICO (pantalla de la Wio) con mas vida: valor grande en el color de su nivel,
      etiqueta del nivel, minimo y maximo con su color, franjas de color por nivel,
      relleno bajo la linea, linea gruesa y punto de "ahora".
  Nuevo en 3.8 (26-nov 14:09):
    - Enojado ("YA!"): el amarillo dura mas; el color pasa a coral despacito (~2 s).
    - microSD: guarda los ajustes (sonido, sensibilidad, ajustes de temperatura y
      humedad, y lo que mide la prueba guiada) en /nimbo/ajustes.txt y los carga al
      prender. Guarda un historial de 1 linea por minuto en /nimbo/historial.csv
      (hora, estado, CO2, PM2.5, PM10, T, HR) y al prender rellena la pantalla
      HISTORICO con las ultimas ~4.6 h. La hora la manda la compu (HORA:) cuando
      nimbo_ia.py esta conectado; sin compu se guarda 0 y los segundos desde que prendio.
      Necesita la libreria "Seeed Arduino FS" (Administrador de bibliotecas).
      Si no la tienes o no hay tarjeta, NIMBO sigue igual (USAR_SD 0 la apaga).
    - AJUSTES > Probar interaccion: IZQ/DER borra lo que midio la prueba (vuelve a fabrica).
    - IA desde la pagina web: la compu manda IA_PENSANDO y NIMBO pone su boquita de
      "pensando" y el aviso "Preguntando a la IA..." mientras espera.
    - DEPURAR apagado (ya no manda lineas "dbg").
  Nuevo en 3.7 (26-nov 14:01: "mucho mejor"):
    - Las barritas son su BOQUITA: se mueven con la cara (mirada, respiracion, brincos)
      y cuando lo inclinas se inclinan igual que los ojos.
    - La boquita tambien sale con la IA: mientras pregunta, tres puntitos que suben y
      bajan ("pensando..."); cuando llega la respuesta, "habla" unos segundos.
    - Reacciones a toques mas largas: el brinquito de cada toque, "si?" (~4 s), "que
      paso?", cosquillas (3.5 s) y "YA!" (3.5 s; los primeros 1.5 s sacude la cabeza).
    - Enojado ("YA!"): el color cambia rapido a coral y se queda toda la reaccion.
  Nuevo en 3.6 (26-nov 13:50: "te escucha pero no se nota; toques mal calibrados: mas
  sensibles o mas notorios"):
    - TE ESCUCHA, muy visible: al empezar hace "mm?" (dos notitas), levanta los ojos y
      ladea la cabeza; mientras hablas los ojos crecen bastante y entre ellos aparece un
      ecualizador de 5 barritas turquesa que sube y baja con tu voz.
    - TOQUES mas sensibles: umbral del acelerometro 0.37 / 0.25 / 0.16 g (baja / media /
      alta; antes 0.50 / 0.37 / 0.25) y acepta golpes de hasta 100 ms (antes 30 ms, los
      toques suaves en la carcasa duran mas).
    - TOQUES mas notorios: cada toque hace brincar y aplastarse un poco los ojos con un
      "tic" audible; 1 toque = ladea la cabeza y "?"; 2 toques = mira a todos lados;
      3-5 = cosquillas mas fuertes; 6+ = "YA!".
    - CO2: se queda como en la 3.5 (ignora picos cortos, se recupera rapido).
  Nuevo en 3.5 (26-nov 13:38: marcaba "CO2 muy alto" con el CO2 en 465):
    - El animo por CO2 usaba solo el promedio de 5 min: si le hablas o respiras cerca,
      el sensor sube mucho unos segundos y el promedio se queda alto varios minutos
      aunque el cuarto ya este bien. Ahora usa el MENOR entre ese promedio y la
      mediana de los ultimos ~30 s. Para empeorar, el CO2 tiene que estar alto de
      forma sostenida (las dos cosas altas); para mejorar basta con que baje de verdad
      unos 30 s. Un respiro cerca ya no lo pone en Alerta, y se recupera rapido.
    - Lo mismo para el bostezo y el cansancio de los ojos.
    - (3.4 verificado en la Wio: toques por hardware OK, "te escucha" OK.)
  Nuevo en 3.4 (26-nov 13:28: "no esta agarrando bien"):
    - TOQUES POR HARDWARE: mientras NIMBO dibuja los ojos (~30 ms de cada cuadro) no puede
      leer el acelerometro, y un toque dura pocos ms: casi todos se perdian (en la prueba
      guiada si salian porque ahi no se dibujan ojos). Ahora el propio acelerometro
      (LIS3DH, funcion "click") detecta el toque y lo GUARDA hasta que NIMBO lo lee.
      Si no responde, usa el metodo de antes (lo dice por USB al arrancar).
    - DEPURAR 1: cada 200 ms manda por USB una linea "dbg,..." con microfono, fondo,
      umbral de voz, contador de voz, si te esta escuchando, golpe, energia y toques.
      Se ve en el Monitor Serie (el panel/IA la ignora). Para mandarsela a Claude.
  Nuevo en 3.3 (prueba del 26-nov 11:45: "con toques solo se marea", "no detecta bien la voz"):
    - Mareo solo con sacudida SOSTENIDA (~0.7 s): la sacudida se mide con el promedio de
      cada 40 ms (un golpe corto casi no cuenta) y con una "energia" que baja rapido.
      Los toquecitos (aunque muevan un poco la carcasa) ya no lo marean ni se cancelan.
    - Te escucha de forma que se note: al empezar a hablar levanta la mirada y ladea la
      cabeza; mientras hablas los ojos crecen ~15-35 % y brincan con tu voz; al callarte
      asiente con un saltito ("entendido").
    - El ruido de fondo casi no sube mientras hablas (antes tu voz se volvia "fondo").
    - Susto: minimo 1800/1500/1250 (baja/media/alta). Prueba 26-nov 11:45: silencio 421
      (p90 598), voz 1214, palmada 2138.
  Nuevo en 3.2 (valores de la prueba guiada del 26-nov, en un salon con gente):
    - Microfono: silencio ~800, voz ~1800, palmada ~3000 (12 bits, ventana 5 ms).
      Voz = 1.6 veces el ruido de fondo; susto = 2.8 veces el fondo y nunca menos de 1700
      (sensibilidad media). Asi funciona igual en casa (fondo ~100) que en un salon.
    - El ruido de fondo sigue a lo "normal" del lugar, no solo a lo mas bajo.
    - Toques: umbral 0.45 g (la prueba midio toques de ~1.3 g; reposo ~0.14 g).
    - Inclinacion confirmada: derecha -25 => TILT_SIGNO_X -1; atras -77 => TILT_SIGNO_Y 1.
    - Prueba guiada: compara contra el silencio "tipico" (percentil 90), no contra el
      pico mas alto; en un lugar ruidoso ya no dice "no te oye" por un ruido suelto.
  Nuevo en 3.1 (lo que salio en la prueba del 25-nov):
    - Inclinacion: signo corregido (se iba al lado contrario): TILT_SIGNO_X = -1.
    - Toques: ya no se cancelan si tocas rapido. NIMBO cuenta la serie y reacciona
      al terminar: 1 = "si?", 2 = ladea la cabeza, 3-5 = cosquillas, 6+ = "YA!"
      (enojado de verdad: sacude la cabeza, entrecierra los ojos y grune).
      Cada toque se nota al instante con un brinquito.
    - Caricias (A) variadas: se mece, se acurruca o brinca de gusto (nunca la misma dos
      veces seguidas); cada 4 caricias seguidas se "derrite".
    - PRUEBA GUIADA (AJUSTES > Probar interaccion, o CLIC en TEST): te pide silencio, hablar,
      una palmada, toquecitos e inclinarlo; mide todo y ajusta solo los umbrales y los
      signos. Al final muestra los numeros (mandaselos a Claude para dejarlos fijos).
    - Humedad: ajuste de -15 puntos (25-nov, contra el medidor de la usuaria). AJUSTES
      muestra la humedad y la temperatura actuales junto a su ajuste, para afinarlas.
    - PM2.5: Feliz < 9 ug/m3 (antes 5), igual que "Bueno" de la EPA. El resto no cambia.
    - El aire tambien se nota en la cara: si empeora un nivel se sobresalta un momento;
      con un pico de particulas (humo, cocina) tose.
  Nuevo en 3.0 (arregla la interaccion de la 2.9 con lo medido el 25-nov):
    - Feliz solo con CO2 < 650 ppm (igual que la 2.8b; la 2.9 tenia 800).
    - INCLINARLO: los ojos "resbalan" hacia el lado que queda abajo y se quedan
      nivelados con el horizonte (un ojo sube, el otro baja). Antes el efecto era
      igual de chico que la mirada al azar y no se notaba. "Derecho" ya no se toma
      al prender: es fijo, medido en la carcasa (gravedad en +X).
      Si lo dejas chueco mas de 3 s pone cara de preocupado; al enderezarlo, suspira.
    - TOQUECITOS: el acelerometro se lee cada ~4 ms (antes cada 40 ms y se perdian).
      1-2 toques = "si?"; 3-5 seguidos = contento; 6 o mas = "ya, ya!" (enojo breve).
    - TE ESCUCHA: el microfono se mide en ventanas de 5 ms (antes ~1 ms, muy corto para
      la voz). Voz y sorpresa se comparan con el ruido de fondo, que se ajusta solo:
      en un lugar ruidoso hace falta mas volumen para asustarlo. Mientras te escucha,
      los ojos laten con tu voz.
    - BOTON A en OJOS = CARICIA: cierra los ojitos contento (^ ^). Varias caricias
      seguidas = contento mas tiempo. Fuera de OJOS, A sigue regresando a OJOS.
    - SE DUERME si nadie le hace caso 20 min (solo con aire Atento o mejor):
      bosteza, cierra los ojos y respira lento. Despierta con cualquier toque, voz,
      inclinacion o boton, y te saluda. Si el aire se pone feo, despierta solo.
    - TE SALUDA si regresas despues de 5 min sin hacerle caso.
    - TEST (A largo) muestra inclinacion, nivel de microfono, fondo y umbrales,
      y el golpe mas fuerte, para afinar los valores [Medir].
  Nuevo en 2.9:
    - Inclinarlo: los ojos se van hacia el lado al que lo inclinas; boca abajo se asusta.
      (Toma como "derecho" la posicion en la que esta al entrar a OJOS.)
    - Toquecito: un golpe suave en la carcasa y te mira con doble parpadeo ("si?").
    - Te escucha: si hablas cerca unos segundos, te mira con ojos un poco mas grandes.
      Solo mide volumen; no graba nada.
    - Gestos desde la IA: la compu puede mandar  GESTO:guino / saltito / ladeo /
      alrededor / sorpresa / feliz  y NIMBO lo hace.
    - Temperatura: ajuste del AHT21 de -9.6 C (medido 25-nov dentro de la carcasa:
      termometro 21.7 C vs AHT21 31.3 C). AJUSTES permite hasta -12.
    - Si al inclinarlo los ojos se van al lado contrario, cambia TILT_SIGNO_X a -1.
  Nuevo en 2.8 (los umbrales de CO2 NO cambian):  Nuevo en 2.8 (los umbrales de CO2 NO cambian):
    - Cara de Atento (CO2 1000-1500 ppm) mas amable: parpado de arriba casi abierto.
    - Cansancio: los ojos empiezan a cerrarse desde 800 ppm (antes 600) y
      se cierran como maximo 15 % a 1400 ppm (antes 22 % a 1000).
  Nuevo en 2.7:  Nuevo en 2.7:
    - Ojos con el diseno de antes: sin brillito y sin estirarse al moverse.
    - Transiciones mas lentas y suaves (~0.8 s entre caras) y rebotes mas chicos.
    - Se quedan los gestos sueltos, la mirada "pensando" y el saltito de la IA.
    - Los puntitos de arriba muestran solo DATOS, HISTORICO y AJUSTES (en OJOS no hay cabecera).
  De la 2.6:
    - AJUSTES con 4 opciones: Sonido, Sensibilidad, Ajuste de temp., Calibrar CO2.
      Promedio (5 min) e Historico (1 punto/min) quedan fijos.
    - Sensibilidad = ruido + sacudida en una sola opcion (apagada/baja/media/alta).
      Por defecto BAJA; la sorpresa por ruido pide mas volumen y espera 8 s entre una y otra.
    - Gestos sueltos cada 15-40 s (guino, mirar alrededor, ladear, saltito si esta contento).
    - Con la IA: mientras pregunta, mira hacia arriba (pensando); al llegar la
      respuesta, da un saltito y te mira.
  Nuevo en 2.5: NIMBO recibe mensajes de IA desde la compu (programa nimbo_ia.py).
    - La compu manda por USB una linea  IA:texto  o  IA:linea1|linea2
    - El texto aparece bajo los ojos (2 lineas, turquesa) durante 15 min.
    - CLIC del joystick en OJOS = pedirle un consejo nuevo a la IA.
    - Sin compu o sin internet, NIMBO sigue igual que la 2.4 (sus propios consejos).
    - El color y la cara siguen saliendo de las reglas, no de la IA.
    - Falla de sensor: manda el aviso de la regla. Alerta: la regla, hasta que
      llegue un mensaje de IA nuevo (la compu pregunta en cuanto cambia el estado).
  Nuevo en 2.4: las reacciones tambien se transforman.
    Sorpresa (ruido fuerte o caida) = el ojo crece rapido con un rebote grande.
    Mareo (sacudida) = el giro empieza pequeno, crece y al final se apaga suave.
  Lo de la 2.3: cada cara es un "ojo con parpados" (alto, ancho, parpado de arriba,
  inclinacion de ceja, parpado de abajo y color). Al cambiar de cara, el ojo se
  TRANSFORMA poco a poco (~0.4 s) en vez de saltar; el color tambien se mezcla.
    Feliz      = lima, parpado de abajo curvo (ojos sonrientes ^ ^)
    Tranquilo  = lima, ovalo
    Atento     = marfil, parpados un poco caidos
    Preocupado = coral, cejas hacia arriba en el centro (preocupacion)
    Alerta     = coral, cejas hacia abajo en el centro (enojo) + temblor
  Lo de la 2.2:
    - Mirada con resorte (se pasa un poco y regresa) + micro-movimientos.
    - Respiracion: los ojos suben y bajan muy suave.
    - Parpadeo con "aplastado" y transicion con rebote al cambiar de cara.
    - Cansancio progresivo: con CO2 entre 600 y 1000 ppm los ojos se cierran un poco
      y parpadean mas seguido.
    - Bostezo cuando el CO2 promedio cruza 1000 ppm (maximo 1 cada 20 min).
    - Suspiro de alivio cuando el aire mejora un nivel o se detecta ventana abierta.
    - Temblor en Alerta (1 s cada 5 s).
    - Demo: en OJOS, joystick ARRIBA = suspiro, ABAJO = bostezo.
  Cambios de la 2.1:
    - Fuera: cara de corazon, sensor de luz (modo noche), opcion de fondo verde.
    - Lienzo de 320x184 a 320x160: libera ~15 KB de RAM para la microSD.
    - DATOS en 2 paginas (joystick ARR/ABA): 1 = CO2, PM2.5, T, HR, PM10, PM1
                                             2 = gas estimado (AQI, TVOC, eCO2)
    - TEST fuera del ciclo: se abre manteniendo A 1 segundo.

  BOTONES (izq -> der):  A  B  C
    A corto = en OJOS caricia; en otra pantalla volver a OJOS · A largo (1 s) = TEST
    B       = silenciar (en TEST da un pitido)
    C       = siguiente pantalla: OJOS -> DATOS -> HISTORICO -> AJUSTES -> OJOS
  JOYSTICK, segun la pantalla:
    OJOS      IZQ/DER = cara manual 15 s · CLIC = consejo / pedir a la IA · ARR = resumen de hoy · ABA = bostezo
    DATOS     ARR/ABA = cambiar pagina
    HISTORICO cualquier direccion = cambiar variable
    AJUSTES   ARR/ABA = elegir · IZQ/DER = cambiar · CLIC 3 s en "Calibrar CO2"
              CLIC en "Probar interaccion" = prueba guiada (tambien CLIC en TEST)
  Sin tocar nada 2 min, vuelve sola a OJOS.

  Los ajustes NO se guardan al apagar (llegara con la microSD).
  CODIGO NO COMPILADO EN ARDUINO: revisado con g++/clang y cabeceras simuladas.
*/

#include <Wire.h>
#include <TFT_eSPI.h>
#include <LIS3DHTR.h>
#include "SparkFun_SCD30_Arduino_Library.h"
#include "SparkFun_ENS160.h"
#include "Adafruit_PM25AQI.h"
#define USAR_SD 1                  // 3.8: 0 si no tienes la libreria "Seeed Arduino FS"
#if USAR_SD
#include <Seeed_FS.h>
#include "SD/Seeed_SD.h"
#endif

TFT_eSPI tft;
TFT_eSprite spr = TFT_eSprite(&tft);
LIS3DHTR<TwoWire> lis;
SCD30 scd30;
SparkFun_ENS160 ens;
Adafruit_PM25AQI pms = Adafruit_PM25AQI();

// =====================================================================
//  PANTALLA
// =====================================================================
#define SWAP_RB             0
#define INVERTIR_PANTALLA   0
#define USAR_FUENTE_GFX     1    // logo con FreeSansBold24pt; 0 si da error
#define FUENTE_GRANDE       6    // numeros grandes de DATOS. Si no se ven, pon 4.

#define FIX(c) (SWAP_RB ? (uint16_t)((((c) & 0x001F) << 11) | ((c) & 0x07E0) | (((c) >> 11) & 0x001F)) : (uint16_t)(c))

// Paleta "Living Atmosphere"
#define COL_NEGRO      FIX(0x0000)
#define COL_MARFIL     FIX(0xEF5B)   // #E9E9D9
#define COL_LIMA       FIX(0xD78B)   // #D4F05A
#define COL_TURQUESA   FIX(0x8E58)   // #8ECBC0
#define COL_CORAL      FIX(0xF348)   // #F26B45
#define COL_ISOBARA    FIX(0x2287)   // #24503F (lineas, cosas apagadas)
#define COL_PANEL      FIX(0x1226)   // #164536 (fondo "verde")

// =====================================================================
//  VALORES DE ARRANQUE (se pueden cambiar en vivo en AJUSTES)
// =====================================================================
#define AHT_OFFSET_T       -9.6f     // 25-nov en la carcasa: termometro 21.7 C vs AHT21 31.3 C [Medir de nuevo]
#define HR_AJUSTE          -15.0f    // 25-nov: NIMBO marcaba ~15 puntos de mas contra el medidor [Medir de nuevo]
#define PROMEDIO_INICIAL   1         // 0 = 20 s (para probar), 1 = 5 min   (ya no esta en AJUSTES)
#define HIST_INICIAL       1         // 0 = 1 punto cada 10 s (~46 min), 1 = cada 1 min (~4.6 h)
#define SENS_INICIAL       1         // sensibilidad (ruido + sacudida): 0 apagada, 1 baja, 2 media, 3 alta
#define SORPRESA_COOL_MS   8000UL    // minimo entre dos sorpresas por ruido
// --- inclinacion (3.0) ---
// Gravedad en reposo dentro de la carcasa (diagnostico 25-nov: X +1.05, Y 0.00, Z 0.02).
#define TILT_REF_X         1.0f
#define TILT_REF_Y         0.0f
#define TILT_REF_Z         0.0f
#define TILT_SIGNO_X       -1        // 26-nov: prueba guiada, derecha = -25 => -1 (confirmado)
#define TILT_SIGNO_Y       1         // 26-nov: prueba guiada, atras = -77 => 1 (confirmado)
#define TILT_MUERTO        5.0f      // grados que se ignoran (ruido en reposo ~2)
#define CHUECO_ON          15.0f     // grados para que se sienta "chueco"
#define CHUECO_OFF         8.0f      // grados para que se sienta derecho otra vez
#define CHUECO_MS          3000UL    // chueco tanto tiempo = cara de preocupado
// --- toques (3.0) ---
#define TAP_THR            0.45f     // 26-nov: toques medidos ~1.3 g, reposo ~0.14 g
#define TAP_REBOTE_MS      250UL     // 4.1: un mismo golpe "rebota" en la carcasa: se cuenta una vez
#define SERIE_FIN_MS       700UL     // sin toques tanto tiempo = termino la serie
#define YA_TOQUES          4         // 4.2: 4 toques seguidos = "YA!"
// --- microfono (3.0) ---
#define MIC_VENTANA_US     5000UL    // ventana de medicion (cubre 1-2 ondas de voz)
#define VOZ_MARGEN         30        // ademas del factor, la voz debe pasar el fondo por esto [Medir]
#define ESCUCHA_MS         3000UL    // cuanto te sigue mirando despues de que dejas de hablar
// --- compania (3.0) ---
#define SUENO_MS           1200000UL // 20 min sin hacerle caso: se duerme
#define SALUDO_MS          300000UL  // 5 min sin hacerle caso: al regresar te saluda

// =====================================================================
//  REACCIONES
// =====================================================================
#define SHAKE_SOSTENIDO_MS 700   // 3.3: la sacudida tiene que durar esto para marearlo
#define INPUT_IGNORE_MS   600       // 4.1: despues de un boton no se cuentan toques ni sustos
#define FREEFALL_G        0.35
#define FREEFALL_SAMPLES  3
#define SHAKE_DURATION    2200
#define NOISE_DURATION    1300
#define DEBUG_ACCEL       0
#define DEPURAR           0      // 1 = linea "dbg,..." cada 200 ms en OJOS (Monitor Serie)
const float SHAKE_THR[4] = {99.0f, 1.3f, 0.9f, 0.6f};   // g (apagada, baja, media, alta)
// 26-nov (prueba guiada): silencio ~800, voz ~1800, palmada ~3000 en un salon con gente; en casa el fondo es ~100
const int   NOISE_THR[4] = {0, 1800, 1500, 1250};       // sorpresa: minimo absoluto (3.3; palmada 26-nov = 2138)
const float SORP_FACTOR[4] = {0, 3.2f, 2.8f, 2.4f};     // sorpresa: veces el ruido de fondo (el mayor de los dos manda)
const float VOZ_FACTOR[4]  = {0, 1.8f, 1.6f, 1.4f};     // voz: veces el ruido de fondo

// =====================================================================
//  SENSORES DE AIRE
// =====================================================================
#define I2C_HZ             50000UL
#define AHT_ADDR           0x38
#define AHT_CHECK_CRC      1
#define ALTITUD_M          2140
#define PM_USAR_ATM        0
#define AQI_PUEDE_ALERTAR  0
#define CO2_FRC_PPM        420       // valor al calibrar CO2 en aire exterior
#define LOG_CADA_MS        10000UL
#define ALERTA_REPETIR_MS  600000UL
#define MANUAL_MS          15000UL
#define AVISO_MS           6000UL    // cuanto dura el aviso bajo los ojos
#define AVISO_AUTO         1         // 1 = aviso automatico cuando cambia el estado
#define VOLVER_OJOS_MS     120000UL
#define A_LARGO_MS         1000UL    // mantener A para abrir TEST
#define IA_VIGENTE_MS      900000UL  // un mensaje de IA se muestra 15 min
#define IA_ESPERA_MS       12000UL   // tiempo maximo esperando respuesta al CLIC

#define PMS_CALENTAR_MS    30000UL
#define SCD_CALENTAR_MS    10000UL
#define SCD_MIN_LECTURAS   3
#define ENS_WARMUP_MS      180000UL
#define ENS_STARTUP_MS     3600000UL
#define TIMEOUT_DATOS_MS   15000UL
#define REINTENTO_MS       15000UL

const float U_CO2[4]  = {650, 1000, 1500, 2000};   // Feliz < 650 (25-nov)
const float U_PM25[4] = {9, 15, 35, 55};   // 3.1: Feliz < 9 (EPA "Bueno" 2024); 35 y 55 = cortes EPA
const float U_PM10[4] = {15, 45, 100, 150};
const float U_AQI[4]  = {1.5, 2.5, 3.5, 4.5};

// =====================================================================
//  BOTONES
// =====================================================================
#define PIN_BTN_A  WIO_KEY_C   // izquierda
#define PIN_BTN_B  WIO_KEY_B   // centro
#define PIN_BTN_C  WIO_KEY_A   // derecha

enum { BT_A, BT_B, BT_C, BT_IZQ, BT_DER, BT_ARR, BT_ABA, BT_CLIC, NBTN };
const uint8_t BTN_PINS[NBTN] = {PIN_BTN_A, PIN_BTN_B, PIN_BTN_C, WIO_5S_LEFT, WIO_5S_RIGHT,
                                WIO_5S_UP, WIO_5S_DOWN, WIO_5S_PRESS};

// =====================================================================
//  TIPOS
// =====================================================================
enum Screen { SC_OJOS, SC_DATOS, SC_HIST, SC_AJUSTES, SC_TEST, SC_PRUEBA, SC_N };
const int SC_CICLO = SC_TEST;   // pantallas que recorre el boton C (TEST queda fuera)
enum Expr { E_NORMAL, E_HAPPY, E_ATENTO, E_WORRIED, E_ANGRY, E_COUNT, E_SURPRISED, E_DIZZY, E_MOLESTO };
enum SensSt { SS_AUSENTE, SS_CALENTANDO, SS_OK, SS_CAIDO };
enum Estado { ST_ALERTA, ST_PREOCUPADO, ST_ATENTO, ST_TRANQUILO, ST_FELIZ, ST_CALENTANDO, ST_CAIDO };

struct Ema { float v; uint32_t t; bool ok; };
struct Var {
  const char *nombre; const char *unidad; const float *u; float margen;
  Ema ema; int8_t nivel;
};

#define HIST_N      276              // un punto por pixel de la grafica
#define HIST_VACIO  (-32768)
struct Serie {
  const char *nombre; const char *unidad; uint8_t escala; const float *u;
  int16_t v[HIST_N]; float suma; uint16_t n;
};

const char *const SS_TXT[]  = {"ausente", "calentando", "ok", "caido"};
const char *const EST_TXT[] = {"Alerta", "Preocupado", "Atento", "Tranquilo", "Feliz", "Calentando", "Sensor caido"};
const char *const SC_TXT[]  = {"OJOS", "DATOS", "HISTORICO", "AJUSTES", "TEST", "PRUEBA GUIADA"};

// =====================================================================
//  ESTADO DEL PROGRAMA
// =====================================================================
Screen scr = SC_OJOS;
Expr expr = E_NORMAL, exprBeforeReact = E_NORMAL, exprAnimo = E_NORMAL;
uint32_t manualUntil = 0;
bool mute = false;
bool accelOk = false;

// ajustes en vivo
uint8_t ajProm = PROMEDIO_INICIAL, ajHist = HIST_INICIAL;
uint8_t ajSens = SENS_INICIAL;
float offsetT = AHT_OFFSET_T;
float offsetHR = HR_AJUSTE;
// valores que la prueba guiada puede cambiar en vivo
int8_t signoX = TILT_SIGNO_X, signoY = TILT_SIGNO_Y;
float tapThr = TAP_THR;
bool calHecha = false, vozSorda = false;
float vozFacCal = 0, sorpFacCal = 0, sorpPisoCal = 0;   // 0 = usar los de fabrica
const uint16_t colFondo = COL_NEGRO;   // fondo negro fijo

// geometria: el sprite (lienzo) ocupa todo el ancho; arriba cabecera, abajo pie
const int SPR_W = 320;
const int SPR_H = 160;              // antes 184: 160 libera ~15 KB de RAM
const int SPR_Y = 40;
const int PIE_Y = SPR_Y + SPR_H;   // 200

float lookX = 0, lookY = 0, tgtX = 0, tgtY = 0;
float openAmt = 1.0;

// animacion (2.2)
#define SUSPIRO_MS       900
#define BOSTEZO_MS       2200
#define BOSTEZO_CADA_MS  1200000UL   // 20 min
float escX = 1, escY = 1;            // escala de los ojos en este cuadro
int offX = 0, offY = 0;              // desplazamiento extra (respiracion, temblor)
float lookVX = 0, lookVY = 0;        // velocidad de la mirada (resorte)
float cansancio = 1.0f;              // 1 = bien abiertos; baja con el CO2
uint32_t reactStartMs = 0;
float popAmp = 0.08f;                // fuerza del rebote del ultimo cambio
uint32_t nextMicro = 0, popStart = 0, suspiroStart = 0, bostezoStart = 0, bostezoCool = 0, ventanaVista = 0;
// gestos sueltos (2.6)
enum Gesto { G_NADA, G_GUINO, G_ALREDEDOR, G_LADEO, G_SALTITO };
Gesto gesto = G_NADA;
void iniciarGesto(Gesto g, uint32_t now);
void startReaction(Expr e, uint32_t duration);
void setExpr(Expr e, bool sound);
void iniciarToque(uint32_t now);
void iaGesto(const char *g, uint32_t now);
// interaccion (3.0)
float gx = 0, gy = 0, gz = 0;            // gravedad filtrada
bool gOk = false, volteado = false, dobleParpadeo = false;
float tiltRefAng = 0, tiltRefFrente = 0; // angulos de la posicion "derecha" (de TILT_REF_*)
float tiltDeg = 0, frenteDeg = 0;        // inclinacion medida (grados)
float tiltS = 0, frenteS = 0;            // suavizada, la que usan los ojos
bool chueco = false, incomodo = false, inclinado = false;
uint32_t chuecoDesde = 0;
// acelerometro rapido (para no perder toques)
float acX = 0, acY = 0, acZ = 0, acDeltaMax = 0, golpePico = 0;
bool acPrev = false;
uint32_t acUlt = 0, golpePicoT = 0;
uint32_t tapCool = 0, escuchaHasta = 0, ultGolpe = 0, serieUlt = 0;
uint8_t serieToques = 0;
uint8_t caricias = 0, animTipo = 0, animPrev = 0; uint32_t ultCaricia = 0, animStart = 0, animDur = 0;
uint32_t tosStart = 0; bool tosPend = false, empeoroPend = false;
// microfono
int micAhora = 0;
float ruidoBase = 0, vozNivel = 0;
bool vozAhora = false;
int vozCont = 0;
// compania
bool durmiendo = false;
float dormirAmt = 0;                     // 0 = despierto, 1 = ojos cerrados
uint32_t ultimaInteraccion = 0;
uint32_t gestoStart = 0, proxGesto = 20000;
uint8_t gestoLado = 0;
float gEsc[2] = {1, 1}, gDy[2] = {0, 0}, gAbre[2] = {1, 1};   // efecto del gesto en cada ojo
const uint16_t GESTO_MS[5] = {0, 360, 2600, 2600, 700};   // 3.7: alrededor y ladeo mas largos
bool co2Alto = false;
Estado estadoAnim = ST_CALENTANDO;

// ojo parametrico (2.3): cada cara es un juego de numeros que se transforman
struct Ojo {
  float w, h;       // semiancho y semialto (px)
  float top;        // parpado de arriba: 0 = abierto, 1 = cerrado
  float slope;      // inclinacion de ceja (px): + = baja hacia el centro (enojo), - = sube (preocupacion)
  float bot;        // parpado de abajo curvo: 0 = nada, 1 = ojo sonriente
  float dy;         // desplazamiento vertical del ojo
  float r, g, b;    // color (canales RGB565 como float)
};
Ojo ojo = {26, 38, 0, 0, 0, 0, 0, 0, 0};
bool ojoListo = false;

bool blinking = false;
uint32_t blinkStart = 0, nextBlink = 2000, nextLook = 1000, lastFrame = 0;

uint32_t reactUntil = 0, lastSensorRead = 0, noiseCooldown = 0;
float pax = 0, pay = 0, paz = 0;
bool havePrev = false;
uint8_t ffCount = 0;
float energia = 0;                   // 3.3: que tanto se esta moviendo (promedio de 40 ms)
uint16_t shakeMs = 0;
float acSX = 0, acSY = 0, acSZ = 0; uint16_t acN = 0;   // suma de lecturas rapidas de cada ventana
uint32_t escuchaDesde = 0;
uint32_t tapFlash = 0;               // 3.6: brinquito de cada toque
uint32_t iaHablaDesde = 0;           // 3.7: la boquita habla al llegar la respuesta de la IA
// 3.9: boquita en las interacciones, lagrimita, acostado, aburrido
enum { BOCA_NADA, BOCA_SONRISA, BOCA_RISA, BOCA_PREGUNTA, BOCA_GRR, BOCA_O, BOCA_ONDA, BOCA_BOSTEZO };
uint8_t bocaModo = BOCA_NADA; uint32_t bocaDesde = 0, bocaHasta = 0;
void bocaPoner(uint8_t m, uint32_t dur) { bocaModo = m; bocaDesde = millis(); bocaHasta = bocaDesde + dur; }
#define LAGRIMA_MS   3500UL
#define ABURRIDO_MS  480000UL          // 8 min sin hacerle caso: aburrido (zzz)
uint32_t lagrimaDesde = 0, lagrimaProg = 0, proxLagrimaTriste = 0, proxBostezoAburrido = 0;
uint8_t lagrimaLado = 0;
bool aburrido = false, acostado = false, dormidoPorAcostar = false;
uint32_t acostadoDesde = 0;
float aburridoAmt = 0;
#define IA_HABLA_MS 3000UL

bool btnState[NBTN], btnEdge[NBTN], btnSuelta[NBTN];
bool aLargoHecho = false;
uint32_t btnCambio[NBTN];
uint32_t lastInputMs = 0;

uint32_t buzzOff = 0, buzzEndMs = 0;
bool buzzing = false;
uint8_t seqRest = 0;
uint16_t seqFreq = 0, seqOn = 0, seqOff = 0;
int16_t seqStep = 0;
uint32_t seqNext = 0;

// estado del aire
Var vCO2  = {"CO2",   "ppm",   U_CO2,  50.0f, {0, 0, false}, -1};
Var vPM25 = {"PM2.5", "ug/m3", U_PM25, 2.0f,  {0, 0, false}, -1};
Var vPM10 = {"PM10",  "ug/m3", U_PM10, 5.0f,  {0, 0, false}, -1};
Var vAQI  = {"AQI",   "",      U_AQI,  0.3f,  {0, 0, false}, -1};
Estado estado = ST_CALENTANDO;
Var *culpable = nullptr;
uint32_t proxEstado = 0, proxLog = LOG_CADA_MS, proxRecordatorio = 0;
uint32_t lineasLog = 0;
uint32_t avisoHasta = 0;

// SCD30
bool scdCom = false; SensSt scdSt = SS_AUSENTE;
uint16_t scdCO2 = 0; float scdT = NAN, scdRH = NAN;
uint32_t scdT0 = 0, scdUlt = 0, scdNextPoll = 0, scdNextRetry = 0;
uint16_t scdN = 0;
// AHT21
bool ahtCom = false; SensSt ahtSt = SS_AUSENTE;
float ahtTraw = NAN, ahtRHraw = NAN, ahtT = NAN, ahtRH = NAN, ahtRHcomp = NAN;
uint32_t ahtUlt = 0, ahtNext = 0, ahtTrig = 0, ahtNextRetry = 0;
bool ahtMidiendo = false; uint8_t ahtReint = 0;
// ENS160
bool ensCom = false; SensSt ensSt = SS_AUSENTE;
uint8_t ensAddr = 0, ensAQI = 0, ensFallos = 0;
uint16_t ensTVOC = 0, ensECO2 = 0;
int ensFlag = -1;
uint32_t ensT0 = 0, ensNextPoll = 0, ensNextRetry = 0, ensN = 0;
bool ensCompPend = false;
// PMS5003
SensSt pmsSt = SS_CALENTANDO;
uint16_t pm1 = 0, pm25 = 0, pm10 = 0;
uint32_t pmsT0 = 0, pmsUlt = 0, pmsFrames = 0;
uint8_t turnoI2C = 0;

// historico
Serie hCO2 = {"CO2", "ppm", 1, U_CO2, {}, 0, 0};
Serie hPM  = {"PM2.5", "ug/m3", 1, U_PM25, {}, 0, 0};
Serie hT   = {"Temperatura", "C", 10, nullptr, {}, 0, 0};
Serie hRH  = {"Humedad", "%", 10, nullptr, {}, 0, 0};
Serie *const SERIES[4] = {&hCO2, &hPM, &hT, &hRH};
uint16_t histHead = 0, histCount = 0;
uint32_t proxHist = 0;
uint8_t histVar = 0;

// tendencia (1 muestra cada 10 s, 5 min)
#define TR_N 30
float trCO2[TR_N], trPM[TR_N];
uint8_t trHead = 0, trCount = 0;
uint32_t proxTr = 10000;
float pendCO2 = NAN;
uint32_t evVentanaHasta = 0, evHumoHasta = 0, evVentanaCool = 0, evHumoCool = 0;


// pantallas
uint32_t proxPant = 0;
uint8_t ajSel = 0;
bool calibHecho = false;
char calibMsg[34] = "";
uint32_t calibMsgHasta = 0;

// mensajes de IA (llegan por USB desde la compu)
char iaTxt[100] = "";
uint32_t iaHasta = 0;          // hasta cuando se muestra
uint32_t iaRecibidoMs = 0;     // cuando llego el ultimo mensaje
uint32_t estadoDesde = 0;      // cuando cambio el estado por ultima vez
uint32_t iaEsperaHasta = 0;    // esperando respuesta tras un CLIC
bool iaHay = false;
char rxBuf[128];
uint8_t rxN = 0;
bool rxDesborde = false;

// pie de pantalla
char pieTxt[100] = "";
uint16_t pieCol = 0;
int pieProg = -2;
bool pieForzar = true;
uint32_t proxPie = 0;

// =====================================================================
//                           UTILIDADES
// =====================================================================
bool pasado(uint32_t t) { return (int32_t)(millis() - t) >= 0; }

void evento(const char *txt) {
  Serial.print(F("# "));
  Serial.print(millis() / 1000);
  Serial.print(F("s "));
  Serial.println(txt);
}

void agregar(char *dst, size_t cap, const char *src) {
  size_t l = strlen(dst);
  if (l + 1 >= cap) return;
  strncpy(dst + l, src, cap - l - 1);
  dst[cap - 1] = '\0';
}

void fmt1(char *b, size_t n, float v) {
  if (isnan(v)) { snprintf(b, n, "--"); return; }
  int x = (int)floorf(v * 10.0f + 0.5f);
  int ax = x < 0 ? -x : x;
  snprintf(b, n, "%s%d.%d", x < 0 ? "-" : "", ax / 10, ax % 10);
}

void fmtTiempo(char *b, size_t n, uint32_t ms) {
  unsigned long s = (ms + 999UL) / 1000UL;
  if (s >= 60) snprintf(b, n, "%lu:%02lu", s / 60UL, s % 60UL);
  else snprintf(b, n, "%lus", s);
}

float tauProm() { return ajProm ? 300.0f : 20.0f; }
uint32_t histPeriodoMs() { return ajHist ? 60000UL : 10000UL; }

uint16_t colNivel(int8_t n) {
  if (n < 0) return COL_MARFIL;
  if (n >= 3) return COL_LIMA;
  if (n == 2) return COL_MARFIL;
  return COL_CORAL;
}

uint16_t colEstado(Estado e) {
  switch (e) {
    case ST_FELIZ: case ST_TRANQUILO: return COL_LIMA;
    case ST_ATENTO: return COL_MARFIL;
    case ST_CALENTANDO: return COL_TURQUESA;
    default: return COL_CORAL;
  }
}

bool sensorFalla(SensSt s) { return s == SS_AUSENTE || s == SS_CAIDO; }

extern "C" char *sbrk(int incr);
int ramLibre() { char pila; return (int)(&pila - sbrk(0)); }   // aproximado

void drawBar(int x, int y, int w, int h, int value, int maxValue, uint16_t color) {
  int fill = constrain(map(value, 0, maxValue, 0, w), 0, w);
  tft.fillRect(x, y, fill, h, color);
  tft.fillRect(x + fill, y, w - fill, h, COL_ISOBARA);
}

// ---------- botones ----------
void leerBotones(uint32_t now) {
  for (int i = 0; i < NBTN; i++) {
    bool s = (digitalRead(BTN_PINS[i]) == LOW);
    btnEdge[i] = false;
    btnSuelta[i] = false;
    if (s != btnState[i] && now - btnCambio[i] >= 25) {
      btnState[i] = s;
      btnCambio[i] = now;
      lastInputMs = now;
      if (s) btnEdge[i] = true; else btnSuelta[i] = true;
    }
  }
}

// ---------- buzzer ----------
void beep(uint16_t freq, uint16_t ms) {
  tone(WIO_BUZZER, freq);
  buzzing = true;
  buzzOff = millis() + ms;
}
void beepSeq(uint8_t n, uint16_t f, uint16_t on, uint16_t off, int16_t step) {
  seqRest = n; seqFreq = f; seqOn = on; seqOff = off; seqStep = step; seqNext = millis();
}
void buzzerUpdate() {
  if (buzzing && pasado(buzzOff)) { noTone(WIO_BUZZER); buzzing = false; buzzEndMs = millis(); }
  if (seqRest && !buzzing && pasado(seqNext)) {
    beep(seqFreq, seqOn);
    seqFreq = (uint16_t)(seqFreq + seqStep);
    seqRest--;
    seqNext = millis() + seqOn + seqOff;
  }
}
void click() { if (!mute) beep(1500, 15); }

// pico a pico del microfono durante MIC_VENTANA_US (antes 64 lecturas = ~1 ms, muy corto para la voz)
int micNivel() {
  int mn = 4095, mx = 0;
  uint32_t t0 = micros();
  do {
    int v = analogRead(WIO_MIC);
    if (v < mn) mn = v;
    if (v > mx) mx = v;
  } while (micros() - t0 < MIC_VENTANA_US);
  return mx - mn;
}

const float SENS_MULT[4] = {0, 1.15f, 1.0f, 0.85f};   // con valores de la prueba guiada: baja pide mas, alta menos
float umbralVoz() {
  if (vozFacCal > 0) return ruidoBase * vozFacCal * SENS_MULT[ajSens];
  return ruidoBase * VOZ_FACTOR[ajSens] + VOZ_MARGEN;
}
int umbralSorpresa() {
  if (ajSens == 0) return 0;
  float rel, piso;
  if (sorpFacCal > 0) { rel = ruidoBase * sorpFacCal * SENS_MULT[ajSens]; piso = sorpPisoCal * SENS_MULT[ajSens]; }
  else { rel = ruidoBase * SORP_FACTOR[ajSens]; piso = NOISE_THR[ajSens]; }
  return (int)(rel > piso ? rel : piso);
}

// mide y actualiza el ruido de fondo (baja rapido, sube lento: se acostumbra al ruido del lugar)
void medirMic(bool buzzerReciente) {
  micAhora = micNivel();
  if (buzzerReciente) return;                      // el buzzer no cuenta como ruido
  if (ruidoBase <= 0) ruidoBase = micAhora;
  else ruidoBase += (micAhora - ruidoBase) * (micAhora < ruidoBase ? 0.03f : (vozAhora ? 0.001f : 0.008f));   // 3.3: tu voz no se vuelve "fondo"
  float uv = umbralVoz();
  vozAhora = ajSens > 0 && !vozSorda && micAhora > uv;
  float n = vozAhora ? (micAhora - uv) / (uv + 1.0f) : 0.0f;
  if (n > 1) n = 1;
  vozNivel += (n - vozNivel) * 0.3f;
}

// ---------- toques por hardware (3.4): funcion "click" del LIS3DH ----------
// Registros del LIS3DH: WHO_AM_I 0x0F (=0x33), CTRL_REG2 0x21 (bit 2 HPCLICK), CLICK_CFG 0x38,
// CLICK_SRC 0x39 (bit 6 = hubo toque), CLICK_THS 0x3A (bit 7 = se queda guardado hasta leerlo),
// TIME_LIMIT 0x3B, TIME_LATENCY 0x3C, TIME_WINDOW 0x3D. Umbral: 1 = 15.6 mg en +-2 g.
const uint8_t CLICK_THS_SENS[4] = {30, 30, 22, 16};   // 4.1: apagada/baja/media/alta: 0.47 / 0.47 / 0.34 / 0.25 g
uint8_t lisDir = 0;
bool clickHw = false;
uint16_t toquesHw = 0;
float golpeMaxDbg = 0;

uint8_t lisLeer(uint8_t reg) {
  Wire1.beginTransmission(lisDir);
  Wire1.write(reg);
  if (Wire1.endTransmission() != 0) return 0xFF;
  if (Wire1.requestFrom(lisDir, (size_t)1) != 1) return 0xFF;
  return (uint8_t)Wire1.read();
}
void lisEscribir(uint8_t reg, uint8_t v) {
  Wire1.beginTransmission(lisDir);
  Wire1.write(reg);
  Wire1.write(v);
  Wire1.endTransmission();
}
void clickUmbral() { if (clickHw) lisEscribir(0x3A, (uint8_t)(0x80 | CLICK_THS_SENS[ajSens])); }

void clickConfigurar() {
  clickHw = false;
  const uint8_t dirs[2] = {0x18, 0x19};
  for (uint8_t i = 0; i < 2 && !clickHw; i++) { lisDir = dirs[i]; clickHw = (lisLeer(0x0F) == 0x33); }
  if (!clickHw) { evento("toques por hardware: NO (uso el metodo de antes)"); return; }
  uint8_t r2 = lisLeer(0x21);
  if (r2 != 0xFF) lisEscribir(0x21, (uint8_t)(r2 | 0x04));   // filtro pasa-altas solo para el click
  lisEscribir(0x38, 0x15);   // un toque en X, Y o Z
  clickUmbral();
  lisEscribir(0x3B, 40);     // 3.6: el golpe dura menos de 100 ms (400 Hz); los suaves en la carcasa duran mas
  lisEscribir(0x3C, 0);
  lisEscribir(0x3D, 0);
  lisLeer(0x39);             // limpia lo que hubiera
  Serial.print(F("# toques por hardware: SI (LIS3DH en 0x"));
  Serial.print(lisDir, HEX);
  Serial.println(F(")"));
}
bool clickLeer() {           // true si hubo un toque desde la ultima lectura (y lo borra)
  if (!clickHw) return false;
  uint8_t sr = lisLeer(0x39);
  return sr != 0xFF && (sr & 0x40);
}

// acelerometro rapido: llamar seguido; guarda el golpe mas fuerte entre dos lecturas
void acelRapido(uint32_t now) {
  if (!accelOk || now - acUlt < 4) return;
  acUlt = now;
  float x = lis.getAccelerationX();
  float y = lis.getAccelerationY();
  float z = lis.getAccelerationZ();
  if (acPrev) {
    float dx = x - acX, dy = y - acY, dz = z - acZ;
    float d = sqrtf(dx * dx + dy * dy + dz * dz);
    if (d > acDeltaMax) acDeltaMax = d;
    if (d > golpePico || now - golpePicoT > 3000) { golpePico = d; golpePicoT = now; }
  }
  acX = x; acY = y; acZ = z;
  acPrev = true;
  acSX += x; acSY += y; acSZ += z; acN++;
}

float envolver(float a) {
  while (a > 180) a -= 360;
  while (a < -180) a += 360;
  return a;
}

// inclinacion en grados respecto a la posicion "derecha" (TILT_REF_*), sin signo
void angulosCrudos(float x, float y, float z, float &izqDer, float &adelAtras) {
  float plano = sqrtf(x * x + y * y);
  float mag = sqrtf(x * x + y * y + z * z);
  izqDer = (plano > 0.45f) ? envolver(atan2f(y, x) * 57.2958f - tiltRefAng) : 0;
  adelAtras = (mag > 0.5f) ? (asinf(constrain(z / mag, -1.0f, 1.0f)) * 57.2958f - tiltRefFrente) : 0;
}
// con signo: + = el lado derecho de NIMBO queda abajo; + adelante/atras = mira hacia abajo
void angulos(float x, float y, float z, float &izqDer, float &adelAtras) {
  angulosCrudos(x, y, z, izqDer, adelAtras);
  izqDer *= signoX;
  adelAtras *= signoY;
}

bool i2cPresente(uint8_t addr) {
  Wire.beginTransmission(addr);
  return Wire.endTransmission() == 0;
}

void i2cScan() {
  Serial.print(F("# I2C en Wire (pines 3/5):"));
  int n = 0;
  for (uint8_t a = 1; a < 127; a++) {
    if (i2cPresente(a)) {
      Serial.print(F(" 0x"));
      if (a < 16) Serial.print('0');
      Serial.print(a, HEX);
      n++;
    }
  }
  if (n == 0) Serial.print(F(" ninguno"));
  Serial.println();
}

// =====================================================================
//              PROMEDIO + NIVELES + HISTORICO + TENDENCIA
// =====================================================================
void emaAdd(Ema &e, float x, uint32_t now) {
  if (!e.ok) { e.v = x; e.t = now; e.ok = true; return; }
  float dt = (now - e.t) / 1000.0f;
  e.t = now;
  if (dt <= 0) return;
  float a = dt / (tauProm() + dt);
  e.v += a * (x - e.v);
}

int nivelCrudo(const float *u, float x) {
  int n = 4;
  for (int i = 0; i < 4; i++) if (x >= u[i]) n--;
  return n;
}

void varEvaluar(Var &v, bool valido, float xUsar = NAN) {
  if (!valido || !v.ema.ok) { v.nivel = -1; return; }
  float x = isnan(xUsar) ? v.ema.v : xUsar;
  int crudo = nivelCrudo(v.u, x);
  if (v.nivel < 0 || crudo < v.nivel) { v.nivel = crudo; return; }
  int conMargen = nivelCrudo(v.u, x + v.margen);
  if (conMargen > v.nivel) v.nivel = conMargen;
}

void histAdd(Serie &s, float x) { s.suma += x; s.n++; }

void histReset() {
  for (int k = 0; k < 4; k++) {
    for (int i = 0; i < HIST_N; i++) SERIES[k]->v[i] = HIST_VACIO;
    SERIES[k]->suma = 0;
    SERIES[k]->n = 0;
  }
  histHead = 0;
  histCount = 0;
}

void histTick() {
  for (int k = 0; k < 4; k++) {
    Serie &s = *SERIES[k];
    int16_t val = HIST_VACIO;
    if (s.n) {
      float m = s.suma / s.n * s.escala;
      if (m > 32000) m = 32000;
      if (m < -32000) m = -32000;
      val = (int16_t)floorf(m + 0.5f);
    }
    s.v[histHead] = val;
    s.suma = 0;
    s.n = 0;
  }
  histHead = (histHead + 1) % HIST_N;
  if (histCount < HIST_N) histCount++;
}

int16_t histGet(const Serie &s, uint16_t i) {   // i = 0 el mas viejo
  return s.v[(histHead + HIST_N - histCount + i) % HIST_N];
}

// =====================================================================
//          microSD (3.8): ajustes + historial de 1 linea por minuto
// =====================================================================
#define SD_DIR      "/nimbo"
#define SD_AJUSTES  "/nimbo/ajustes.txt"
#define SD_HIST     "/nimbo/historial.csv"
bool sdOk = false;
uint32_t horaBase = 0;          // hora de la compu (segundos desde 1970) menos los segundos encendido; 0 = sin hora
// 4.0: hora local, rutinas del dia, resumen y avisos
long zonaSeg = 0;               // diferencia con UTC en segundos (Puebla: -21600), la manda la compu
uint32_t diaBuenosDias = 0, diaNoche = 0, diaResumen = 0, diaCuenta = 0;
uint16_t minEstado[5] = {0, 0, 0, 0, 0};   // minutos de hoy en Alerta..Feliz
uint8_t ventilacionesHoy = 0;
bool bailePend = false;
char msgTxt[90] = ""; uint16_t msgCol = 0; uint32_t msgHasta = 0;
void mostrarMsg(const char *t, uint32_t ms, uint16_t col) {
  strncpy(msgTxt, t, sizeof(msgTxt) - 1); msgTxt[sizeof(msgTxt) - 1] = '\0';
  msgCol = col; msgHasta = millis() + ms;
}
bool horaOk() { return horaBase != 0; }
uint32_t ahoraLocal() { return (uint32_t)((int64_t)horaBase + millis() / 1000 + zonaSeg); }
int horaLocal() { return horaOk() ? (int)((ahoraLocal() % 86400UL) / 3600UL) : -1; }
uint32_t diaLocal() { return horaOk() ? ahoraLocal() / 86400UL : 0; }
bool esNoche() { int h = horaLocal(); return h >= 22 || (h >= 0 && h < 6); }
bool esManana() { int h = horaLocal(); return h >= 6 && h < 11; }
uint32_t ajustesSucios = 0;     // cuando cambio un ajuste (se guarda 3 s despues)

void sdIniciar() {
#if USAR_SD
  sdOk = SD.begin(SDCARD_SS_PIN, SDCARD_SPI);
  if (!sdOk) { evento("microSD: no hay tarjeta o no responde (sigo sin guardar)"); return; }
  if (!SD.exists(SD_DIR)) SD.mkdir(SD_DIR);
  evento("microSD lista");
#endif
}

void ajustesGuardar() {
#if USAR_SD
  if (!sdOk) return;
  File f = SD.open(SD_AJUSTES, FILE_WRITE);
  if (!f) { evento("microSD: no pude guardar los ajustes"); return; }
  f.print("sonido="); f.println(mute ? 0 : 1);
  f.print("sens="); f.println((int)ajSens);
  f.print("ajusteT="); f.println(offsetT, 1);
  f.print("ajusteHR="); f.println(offsetHR, 0);
  f.print("prueba="); f.println(calHecha ? 1 : 0);
  if (calHecha) {
    f.print("vozFac="); f.println(vozFacCal, 3);
    f.print("sorpFac="); f.println(sorpFacCal, 3);
    f.print("sorpPiso="); f.println(sorpPisoCal, 0);
    f.print("tapThr="); f.println(tapThr, 3);
    f.print("signoX="); f.println((int)signoX);
    f.print("signoY="); f.println((int)signoY);
  }
  f.close();
  evento("ajustes guardados en la microSD");
#endif
}

void ajusteAplicar(const char *k, const char *v) {   // valida cada valor antes de usarlo
  float x = atof(v);
  if (!strcmp(k, "sonido")) mute = (x == 0);
  else if (!strcmp(k, "sens")) { if (x >= 0 && x <= 3) ajSens = (uint8_t)x; }
  else if (!strcmp(k, "ajusteT")) { if (x >= -12 && x <= 3) offsetT = x; }
  else if (!strcmp(k, "ajusteHR")) { if (x >= -30 && x <= 10) offsetHR = x; }
  else if (!strcmp(k, "prueba")) calHecha = (x != 0);
  else if (!strcmp(k, "vozFac")) { if (x > 0 && x < 20) vozFacCal = x; }
  else if (!strcmp(k, "sorpFac")) { if (x > 0 && x < 50) sorpFacCal = x; }
  else if (!strcmp(k, "sorpPiso")) { if (x > 0 && x < 4096) sorpPisoCal = x; }
  else if (!strcmp(k, "tapThr")) { if (x > 0.05f && x < 3) tapThr = x; }
  else if (!strcmp(k, "signoX")) signoX = (x < 0) ? -1 : 1;
  else if (!strcmp(k, "signoY")) signoY = (x < 0) ? -1 : 1;
}

void ajustesCargar() {
#if USAR_SD
  if (!sdOk || !SD.exists(SD_AJUSTES)) return;
  File f = SD.open(SD_AJUSTES, FILE_READ);
  if (!f) return;
  char lin[48]; uint8_t n = 0;
  while (true) {
    int c = f.available() ? f.read() : -1;
    if (c < 0 || c == '\n' || c == '\r') {
      if (n) {
        lin[n] = '\0';
        char *eq = strchr(lin, '=');
        if (eq) { *eq = '\0'; ajusteAplicar(lin, eq + 1); }
        n = 0;
      }
      if (c < 0) break;
    } else if (n < sizeof(lin) - 1) lin[n++] = (char)c;
  }
  f.close();
  if (!calHecha) { vozFacCal = sorpFacCal = sorpPisoCal = 0; }
  evento("ajustes cargados de la microSD");
#endif
}

// una linea del historial: hora,segundos,estado,co2,pm25,pm10,t,rh  ->  al buffer de HISTORICO
bool historialLinea(char *lin) {
  if (lin[0] < '0' || lin[0] > '9') return false;   // cabecera u otra cosa
  char *campo[8];
  int k = 0;
  campo[k++] = lin;
  for (char *p = lin; *p && k < 8; p++) if (*p == ',') { *p = '\0'; campo[k++] = p + 1; }
  if (k < 8) return false;
  const int idx[4] = {3, 4, 6, 7};                   // co2, pm25, t, rh (mismo orden que SERIES)
  for (int j = 0; j < 4; j++) {
    Serie &s = *SERIES[j];
    const char *c = campo[idx[j]];
    s.v[histHead] = (*c == '\0') ? HIST_VACIO : (int16_t)floorf(atof(c) * s.escala + 0.5f);
  }
  histHead = (histHead + 1) % HIST_N;
  if (histCount < HIST_N) histCount++;
  return true;
}

void historialCargar() {
#if USAR_SD
  if (!sdOk || !SD.exists(SD_HIST)) return;
  File f = SD.open(SD_HIST, FILE_READ);
  if (!f) return;
  uint32_t tam = f.size();
  const uint32_t LEER = (uint32_t)HIST_N * 70UL;     // bastante para las ultimas ~276 lineas
  if (tam > LEER) { f.seek(tam - LEER); while (f.available() && f.read() != '\n') {} }
  char lin[96]; uint8_t n = 0; uint16_t cargadas = 0;
  while (true) {
    int c = f.available() ? f.read() : -1;
    if (c < 0 || c == '\n' || c == '\r') {
      if (n) { lin[n] = '\0'; if (historialLinea(lin)) cargadas++; n = 0; }
      if (c < 0) break;
    } else if (n < sizeof(lin) - 1) lin[n++] = (char)c;
  }
  f.close();
  Serial.print(F("# historial: "));
  Serial.print(cargadas);
  Serial.println(F(" minutos cargados de la microSD"));
#endif
}

void historialEscribir() {   // se llama cada minuto, despues de histTick
#if USAR_SD
  if (!sdOk) return;
  File f = SD.open(SD_HIST, FILE_APPEND);
  if (!f) return;
  if (f.size() == 0) f.println("hora,segundos,estado,co2,pm25,pm10,t,rh");
  uint32_t seg = millis() / 1000;
  f.print(horaBase ? horaBase + seg : 0UL); f.print(',');
  f.print(seg); f.print(',');
  f.print(EST_TXT[estado]); f.print(',');
  uint16_t i = (histHead + HIST_N - 1) % HIST_N;     // el punto que se acaba de guardar
  int16_t co2 = hCO2.v[i], pm = hPM.v[i], t = hT.v[i], rh = hRH.v[i];
  if (co2 != HIST_VACIO) f.print(co2);
  f.print(',');
  if (pm != HIST_VACIO) f.print(pm);
  f.print(',');
  if (vPM10.ema.ok) f.print((int)(vPM10.ema.v + 0.5f));
  f.print(',');
  if (t != HIST_VACIO) f.print(t / 10.0f, 1);
  f.print(',');
  if (rh != HIST_VACIO) f.print(rh / 10.0f, 0);
  f.println();
  f.close();
#endif
}

void trPush() {
  trCO2[trHead] = (scdSt == SS_OK) ? (float)scdCO2 : NAN;
  trPM[trHead]  = (pmsSt == SS_OK) ? (float)pm25 : NAN;
  trHead = (trHead + 1) % TR_N;
  if (trCount < TR_N) trCount++;
}

float pendiente(const float *r) {   // regresion lineal, unidades por minuto
  float sx = 0, sy = 0, sxx = 0, sxy = 0;
  int n = 0;
  for (uint8_t i = 0; i < trCount; i++) {
    uint8_t k = (trHead + TR_N - trCount + i) % TR_N;
    if (isnan(r[k])) continue;
    float x = i * (10.0f / 60.0f);
    sx += x; sy += r[k]; sxx += x * x; sxy += x * r[k];
    n++;
  }
  if (n < 6) return NAN;
  float d = n * sxx - sx * sx;
  if (d <= 0) return NAN;
  return (n * sxy - sx * sy) / d;
}

void analizar(uint32_t now) {
  pendCO2 = pendiente(trCO2);

  if (!isnan(pendCO2) && pendCO2 <= -30.0f && pasado(evVentanaCool)) {
    evVentanaHasta = now + 300000UL;
    bailePend = true;                  // 4.0: celebra
    if (ventilacionesHoy < 250) ventilacionesHoy++;
    evVentanaCool = now + 600000UL;
    evento("evento: posible ventana abierta (CO2 baja rapido)");
  }

  if (trCount >= 6) {
    float ultimo = trPM[(trHead + TR_N - 1) % TR_N];
    float minimo = NAN;
    for (uint8_t i = 0; i < trCount; i++) {
      float x = trPM[i];
      if (!isnan(x) && (isnan(minimo) || x < minimo)) minimo = x;
    }
    if (!isnan(ultimo) && !isnan(minimo) && ultimo >= 35 && ultimo - minimo >= 20 &&
        ultimo >= 2.5f * (minimo < 1 ? 1 : minimo) && pasado(evHumoCool)) {
      evHumoHasta = now + 300000UL;
      tosPend = true;                   // 3.1: NIMBO tose
      evHumoCool = now + 600000UL;
      evento("evento: pico de particulas (humo o cocina?)");
    }
  }
}

// consejo del momento (texto corto, sin acentos: las fuentes son ASCII)
const char *consejo(uint16_t &col) {
  static char b[48];
  uint32_t now = millis();
  bool tOk = (ahtSt == SS_OK && !isnan(ahtT));

  col = COL_CORAL;
  if (!pasado(evHumoHasta))                    return "Pico de particulas: humo o cocina?";
  if (vCO2.nivel == 0)                         return "CO2 muy alto: ventila ya";
  if (sensorFalla(scdSt) || sensorFalla(pmsSt) || sensorFalla(ahtSt) || ensSt == SS_CAIDO) {
    strcpy(b, "Sin datos:");
    if (sensorFalla(scdSt)) agregar(b, sizeof(b), " SCD30");
    if (sensorFalla(pmsSt)) agregar(b, sizeof(b), " PMS5003");
    if (sensorFalla(ahtSt)) agregar(b, sizeof(b), " AHT21");
    if (ensSt == SS_CAIDO)  agregar(b, sizeof(b), " ENS160");
    col = COL_CORAL; return b;
  }
  if (vPM25.nivel >= 0 && vPM25.nivel <= 1)    return "Mucho polvo fino: cierra ventanas";
  if (vCO2.nivel == 1)                         return "CO2 alto: abre una ventana";
  col = COL_MARFIL;
  if (estado == ST_CALENTANDO)                 { col = COL_TURQUESA; return "Calentando sensores..."; }
  if (vCO2.nivel == 2 && pendCO2 >= 15)        return "El CO2 sube: pronto toca ventilar";
  if (vCO2.nivel == 2)                         return "Abre la ventana unos minutos";
  if (!pasado(evVentanaHasta))                 { col = COL_LIMA; return "Entra aire fresco. Bien!"; }
  if (vPM25.nivel == 2)                        return "Algo de polvo en el aire";
  if (vPM10.nivel >= 0 && vPM10.nivel <= 2)    return "Polvo grueso en el aire";
  if (tOk && ahtRH < 30)                       return "Aire seco";
  if (tOk && ahtRH > 65)                       return "Humedad alta";
  if (tOk && ahtT > 28)                        return "Hace calor";
  if (tOk && ahtT < 17)                        return "Hace frio";
  if (!isnan(pendCO2) && pendCO2 >= 15 && scdCO2 >= 700) return "El CO2 empieza a subir";
  (void)now;
  col = COL_LIMA;
  return "Todo bien: aire limpio";
}

// =====================================================================
//                           SCD30 (CO2)
// =====================================================================
// ---------- CO2 "efectivo" (3.5): el menor entre el promedio de 5 min y la mediana de ~30 s ----------
#define CO2_MED_N 15                 // el SCD30 da una lectura cada 2 s: 15 = 30 s
uint16_t co2Ult[CO2_MED_N]; uint8_t co2UltN = 0, co2UltI = 0;
float co2Efectivo = NAN;
void co2Guardar(uint16_t c) {
  co2Ult[co2UltI] = c;
  co2UltI = (co2UltI + 1) % CO2_MED_N;
  if (co2UltN < CO2_MED_N) co2UltN++;
}
float co2Mediana() {
  if (co2UltN == 0) return NAN;
  uint16_t t[CO2_MED_N];
  for (uint8_t i = 0; i < co2UltN; i++) t[i] = co2Ult[i];
  for (int i = 1; i < co2UltN; i++) { uint16_t x = t[i]; int j = i - 1; while (j >= 0 && t[j] > x) { t[j + 1] = t[j]; j--; } t[j + 1] = x; }
  return t[co2UltN / 2];
}

bool scdIniciar() {
  if (!i2cPresente(0x61)) return false;
  bool ok = scd30.begin(Wire);
  Wire.setClock(I2C_HZ);
  if (!ok) { evento("SCD30 responde en 0x61 pero begin() fallo"); return false; }
  scd30.setAltitudeCompensation(ALTITUD_M);
  Wire.setClock(I2C_HZ);
  return true;
}

void scdPaso(uint32_t now) {
  if (!scdCom) {
    if (pasado(scdNextRetry)) {
      scdNextRetry = now + REINTENTO_MS;
      if (scdIniciar()) {
        scdCom = true; scdSt = SS_CALENTANDO;
        scdT0 = now; scdUlt = now; scdN = 0; scdNextPoll = now + 1000;
        evento("SCD30 detectado");
      }
    }
    return;
  }
  if (!pasado(scdNextPoll)) return;
  scdNextPoll = now + 1000;

  if (scd30.dataAvailable()) {
    uint16_t c = scd30.getCO2();
    float t = scd30.getTemperature();
    float h = scd30.getHumidity();
    if (c > 0) {
      scdCO2 = c; scdT = t; scdRH = h; scdUlt = now;
      if (scdN < 60000) scdN++;
      if (scdSt != SS_OK && scdN >= SCD_MIN_LECTURAS && now - scdT0 >= SCD_CALENTAR_MS) {
        scdSt = SS_OK;
        evento("SCD30 listo");
      }
      if (scdSt == SS_OK) { emaAdd(vCO2.ema, c, now); histAdd(hCO2, c); co2Guardar(c); }
    }
  }
  if (now - scdUlt > TIMEOUT_DATOS_MS) {
    scdCom = false; scdSt = SS_CAIDO; vCO2.ema.ok = false; co2UltN = 0;
    scdNextRetry = now + REINTENTO_MS;
    evento("SCD30 sin datos");
  }
}

// =====================================================================
//                     AHT21 (T/HR) - no bloqueante
// =====================================================================
uint8_t ahtStatus() {
  if (Wire.requestFrom((uint8_t)AHT_ADDR, (size_t)1) != 1) return 0xFF;
  return (uint8_t)Wire.read();
}

bool ahtIniciar() {
  if (!i2cPresente(AHT_ADDR)) return false;
  uint8_t st = ahtStatus();
  if (st == 0xFF) return false;
  if (!(st & 0x08)) {
    Wire.beginTransmission((uint8_t)AHT_ADDR);
    Wire.write((uint8_t)0xBE); Wire.write((uint8_t)0x08); Wire.write((uint8_t)0x00);
    Wire.endTransmission();
    delay(10);
  }
  return true;
}

uint8_t crc8Aht(const uint8_t *d, uint8_t n) {
  uint8_t c = 0xFF;
  for (uint8_t i = 0; i < n; i++) {
    c ^= d[i];
    for (uint8_t k = 0; k < 8; k++) c = (c & 0x80) ? (uint8_t)((c << 1) ^ 0x31) : (uint8_t)(c << 1);
  }
  return c;
}

float presionVapor(float t) { return 6.112f * expf(17.62f * t / (243.12f + t)); }

void ahtPaso(uint32_t now) {
  if (!ahtCom) {
    if (pasado(ahtNextRetry)) {
      ahtNextRetry = now + REINTENTO_MS;
      if (ahtIniciar()) {
        ahtCom = true; ahtSt = SS_OK; ahtUlt = now; ahtNext = now; ahtMidiendo = false;
        evento("AHT21 detectado");
      }
    }
    return;
  }

  if (!ahtMidiendo) {
    if (!pasado(ahtNext)) return;
    Wire.beginTransmission((uint8_t)AHT_ADDR);
    Wire.write((uint8_t)0xAC); Wire.write((uint8_t)0x33); Wire.write((uint8_t)0x00);
    if (Wire.endTransmission() == 0) { ahtMidiendo = true; ahtTrig = now; ahtReint = 0; }
    else ahtNext = now + 1000;
  } else if (now - ahtTrig >= 85) {
    uint8_t b[7] = {0, 0, 0, 0, 0, 0, 0};
    uint8_t n = Wire.requestFrom((uint8_t)AHT_ADDR, (size_t)7);
    for (uint8_t i = 0; i < n && i < 7; i++) b[i] = (uint8_t)Wire.read();
    if (n >= 6 && (b[0] & 0x80) && ahtReint < 3) { ahtReint++; ahtTrig = now - 65; return; }
    ahtMidiendo = false;
    ahtNext = now + 5000;
    bool ok = (n >= 6) && !(b[0] & 0x80);
#if AHT_CHECK_CRC
    if (ok && (n < 7 || crc8Aht(b, 6) != b[6])) { ok = false; evento("AHT21 CRC incorrecto"); }
#endif
    if (ok) {
      uint32_t rh = ((uint32_t)b[1] << 12) | ((uint32_t)b[2] << 4) | (b[3] >> 4);
      uint32_t rt = (((uint32_t)b[3] & 0x0F) << 16) | ((uint32_t)b[4] << 8) | b[5];
      float h = rh * 100.0f / 1048576.0f;
      float t = rt * 200.0f / 1048576.0f - 50.0f;
      if (h >= 0 && h <= 100 && t > -40 && t < 85) {
        ahtTraw = t; ahtRHraw = h;
        ahtT = t + offsetT;
        float hc = h * presionVapor(t) / presionVapor(ahtT);
        ahtRHcomp = hc;
        ahtRH = constrain(hc + offsetHR, 0.0f, 100.0f);   // 3.1: ajuste contra el medidor
        ahtUlt = now;
        ensCompPend = true;
        histAdd(hT, ahtT);
        histAdd(hRH, ahtRH);
        if (ahtSt != SS_OK) { ahtSt = SS_OK; evento("AHT21 vuelve"); }
      }
    }
  }
  if (now - ahtUlt > TIMEOUT_DATOS_MS) {
    ahtCom = false; ahtSt = SS_CAIDO; ahtMidiendo = false;
    ahtNextRetry = now + REINTENTO_MS;
    evento("AHT21 sin datos");
  }
}

// =====================================================================
//                    ENS160 (AQI / TVOC / eCO2)
// =====================================================================
bool ensIniciar() {
  uint8_t addr = 0;
  if (i2cPresente(0x53)) addr = 0x53;
  else if (i2cPresente(0x52)) addr = 0x52;
  if (!addr) return false;
  bool ok = ens.begin(addr);
  Wire.setClock(I2C_HZ);
  if (!ok) { evento("ENS160 responde pero begin() fallo"); return false; }
  ensAddr = addr;
  ens.setOperatingMode(SFE_ENS160_STANDARD);   // sin RESET: no reinicia el arranque de 1 h
  return true;
}

void ensPaso(uint32_t now) {
  if (!ensCom) {
    if (pasado(ensNextRetry)) {
      ensNextRetry = now + REINTENTO_MS;
      if (ensIniciar()) {
        ensCom = true; ensSt = SS_CALENTANDO; ensT0 = now; ensFallos = 0; ensNextPoll = now + 1000;
        Serial.print(F("# ENS160 detectado en 0x"));
        Serial.println(ensAddr, HEX);
      }
    }
    return;
  }
  if (ensCompPend) {
    ensCompPend = false;
    ens.setTempCompensationCelsius(ahtTraw);
    ens.setRHCompensationFloat(ahtRHraw);
    return;
  }
  if (!pasado(ensNextPoll)) return;
  ensNextPoll = now + 1000;

  if (!i2cPresente(ensAddr)) {
    if (++ensFallos >= 5) {
      ensCom = false; ensSt = SS_CAIDO; vAQI.ema.ok = false;
      ensNextRetry = now + REINTENTO_MS;
      evento("ENS160 no responde");
    }
    return;
  }
  ensFallos = 0;

  int f = ens.getFlags();
  if (ens.checkDataStatus()) {
    ensAQI = ens.getAQI();
    ensTVOC = ens.getTVOC();
    ensECO2 = ens.getECO2();
    ensN++;
  }
  if (f >= 0 && f <= 3) {
    SensSt nuevo = (f == 0) ? SS_OK : (f == 3) ? SS_CAIDO : SS_CALENTANDO;
    if (nuevo != ensSt || f != ensFlag) {
      ensSt = nuevo;
      Serial.print(F("# ENS160 flag "));
      Serial.print(f);
      Serial.print(F(" -> "));
      Serial.println(SS_TXT[ensSt]);
      if (ensSt != SS_OK) vAQI.ema.ok = false;
    }
    ensFlag = f;
  }
  if (ensSt == SS_OK && ensN > 0 && ensAQI >= 1 && ensAQI <= 5) emaAdd(vAQI.ema, ensAQI, now);
}

// =====================================================================
//                          PMS5003 (PM)
// =====================================================================
void pmsPaso(uint32_t now) {
  PM25_AQI_Data d;
  if (pms.read(&d)) {
#if PM_USAR_ATM
    pm1 = d.pm10_env; pm25 = d.pm25_env; pm10 = d.pm100_env;
#else
    pm1 = d.pm10_standard; pm25 = d.pm25_standard; pm10 = d.pm100_standard;
#endif
    pmsFrames++;
    pmsUlt = now;
    if (pmsSt == SS_AUSENTE || pmsSt == SS_CAIDO) {
      pmsSt = SS_CALENTANDO; pmsT0 = now;
      evento("PMS5003 recibiendo datos");
    }
    if (pmsSt == SS_CALENTANDO && now - pmsT0 >= PMS_CALENTAR_MS) {
      pmsSt = SS_OK;
      evento("PMS5003 listo");
    }
    if (pmsSt == SS_OK) {
      emaAdd(vPM25.ema, pm25, now);
      emaAdd(vPM10.ema, pm10, now);
      histAdd(hPM, pm25);
    }
  }
  if (pmsFrames == 0) {
    if (now > 10000 && pmsSt != SS_AUSENTE) {
      pmsSt = SS_AUSENTE;
      evento("PMS5003 sin datos: revisa TX/RX cruzados y 5 V");
    }
  } else if (now - pmsUlt > TIMEOUT_DATOS_MS && pmsSt != SS_CAIDO) {
    pmsSt = SS_CAIDO; vPM25.ema.ok = false; vPM10.ema.ok = false;
    evento("PMS5003 dejo de enviar");
  }
}

void i2cTurno(uint32_t now) {
  switch (turnoI2C++ % 3) {
    case 0: scdPaso(now); break;
    case 1: ensPaso(now); break;
    default: ahtPaso(now); break;
  }
}

// =====================================================================
//                     ESTADO GLOBAL -> EXPRESION
// =====================================================================
Expr exprDeEstado(Estado e) {
  switch (e) {
    case ST_FELIZ:      return E_HAPPY;
    case ST_TRANQUILO:  return E_NORMAL;
    case ST_ATENTO:     return E_ATENTO;
    case ST_PREOCUPADO: return E_WORRIED;
    case ST_ALERTA:     return E_ANGRY;
    case ST_CAIDO:      return E_ATENTO;
    default:            return E_NORMAL;
  }
}

void alertaSonar(uint32_t now) {
  proxRecordatorio = now + ALERTA_REPETIR_MS;
  if (!mute) beepSeq(3, 2200, 140, 120, 0);
}

void estadoActualizar(uint32_t now) {
  {
    float med = co2Mediana();
    co2Efectivo = (vCO2.ema.ok && !isnan(med)) ? (med < vCO2.ema.v ? med : vCO2.ema.v) : NAN;
    varEvaluar(vCO2, scdSt == SS_OK, co2Efectivo);
  }
  varEvaluar(vPM25, pmsSt == SS_OK);
  varEvaluar(vPM10, pmsSt == SS_OK);
  varEvaluar(vAQI,  ensSt == SS_OK);
#if !AQI_PUEDE_ALERTAR
  if (vAQI.nivel == 0) vAQI.nivel = 1;
#endif

  Var *vars[4] = {&vCO2, &vPM25, &vPM10, &vAQI};
  int g = 99;
  Var *c = nullptr;
  for (int i = 0; i < 4; i++) {
    if (vars[i]->nivel >= 0 && vars[i]->nivel < g) { g = vars[i]->nivel; c = vars[i]; }
  }

  Estado nuevo;
  if (c) nuevo = (Estado)g;
  else if (scdSt == SS_CALENTANDO || pmsSt == SS_CALENTANDO || ensSt == SS_CALENTANDO) nuevo = ST_CALENTANDO;
  else nuevo = ST_CAIDO;
  culpable = c;

  if (nuevo != estado) {
    Estado prev = estado;
    estado = nuevo;
    estadoDesde = now;
    Serial.print(F("# "));
    Serial.print(now / 1000);
    Serial.print(F("s estado: "));
    Serial.print(EST_TXT[estado]);
    if (c) { Serial.print(F(" (manda ")); Serial.print(c->nombre); Serial.print(')'); }
    Serial.println();
    if (estado == ST_ALERTA && prev != ST_ALERTA) alertaSonar(now);
    if (estado < prev && prev <= ST_FELIZ && estado != ST_ALERTA) empeoroPend = true;   // 3.1: se sobresalta
#if AVISO_AUTO
    if (estado <= ST_FELIZ && prev != ST_CALENTANDO) avisoHasta = now + AVISO_MS;
#endif
  }
  if (estado == ST_ALERTA && pasado(proxRecordatorio)) alertaSonar(now);

  exprAnimo = exprDeEstado(estado);
}

// =====================================================================
//                         LOG CSV POR SERIE
// =====================================================================
void csvSep() { Serial.print(','); }
void csvFloat(float v, int dec) { csvSep(); if (!isnan(v)) Serial.print(v, dec); }
void csvInt(long v, bool ok) { csvSep(); if (ok) Serial.print(v); }
void csvEma(const Var &v) { csvSep(); if (v.ema.ok) Serial.print(v.ema.v, 1); }

void logCSV(uint32_t now) {
  if (lineasLog % 30 == 0) {
    Serial.println(F("ms,estado,co2,t,rh,scd_t,scd_rh,pm1,pm25,pm10,aqi,tvoc,eco2,ens_flag,"
                     "co2_prom,pm25_prom,pm10_prom,aqi_prom,n_co2,n_pm25,n_pm10,n_aqi,"
                     "st_scd,st_aht,st_ens,st_pms,co2_pend,t_cruda"));
  }
  lineasLog++;

  bool scdDato = scdCom && scdN > 0;
  bool ahtDato = ahtSt == SS_OK && !isnan(ahtT);
  bool pmsDato = pmsFrames > 0 && (pmsSt == SS_OK || pmsSt == SS_CALENTANDO);
  bool ensDato = ensCom && ensN > 0;

  Serial.print(now);
  csvSep(); Serial.print(EST_TXT[estado]);
  csvInt(scdCO2, scdDato);
  csvFloat(ahtDato ? ahtT : NAN, 1);
  csvFloat(ahtDato ? ahtRH : NAN, 1);
  csvFloat(scdDato ? scdT : NAN, 1);
  csvFloat(scdDato ? scdRH : NAN, 1);
  csvInt(pm1, pmsDato); csvInt(pm25, pmsDato); csvInt(pm10, pmsDato);
  csvInt(ensAQI, ensDato); csvInt(ensTVOC, ensDato); csvInt(ensECO2, ensDato);
  csvInt(ensFlag, ensCom && ensFlag >= 0);
  csvEma(vCO2); csvEma(vPM25); csvEma(vPM10); csvEma(vAQI);
  csvInt(vCO2.nivel, vCO2.nivel >= 0);
  csvInt(vPM25.nivel, vPM25.nivel >= 0);
  csvInt(vPM10.nivel, vPM10.nivel >= 0);
  csvInt(vAQI.nivel, vAQI.nivel >= 0);
  csvSep(); Serial.print(SS_TXT[scdSt]);
  csvSep(); Serial.print(SS_TXT[ahtSt]);
  csvSep(); Serial.print(SS_TXT[ensSt]);
  csvSep(); Serial.print(SS_TXT[pmsSt]);
  csvFloat(pendCO2, 1);
  csvFloat(ahtDato ? ahtTraw : NAN, 1);
  Serial.println();
}

// =====================================================================
//                  CABECERA Y PIE (texto fuera del lienzo)
// =====================================================================
void drawHeader() {
  tft.fillRect(0, 0, 320, SPR_Y, colFondo);
  tft.setTextDatum(TL_DATUM);
  tft.setTextPadding(0);
  tft.setTextColor(COL_TURQUESA, colFondo);
  tft.drawString(SC_TXT[scr], 10, 12, 2);
  for (int i = 1; i < SC_CICLO; i++) {   // sin OJOS: en OJOS no hay cabecera
    int x = 320 - 12 - (SC_CICLO - 1 - i) * 12;
    tft.fillCircle(x, 19, 3, i == scr ? COL_LIMA : COL_ISOBARA);
  }
}

void headerDerecha(const char *txt, uint16_t col) {   // texto a la derecha, antes de los puntos
  const int xDer = 320 - 12 - (SC_CICLO - 1) * 12 - 4;
  tft.fillRect(110, 0, xDer - 110 + 2, SPR_Y, colFondo);
  tft.setTextDatum(TR_DATUM);
  tft.setTextPadding(0);
  tft.setTextColor(col, colFondo);
  tft.drawString(txt, xDer, 12, 2);
  tft.setTextDatum(TL_DATUM);
}

void recortarAncho(char *t);
void pie(const char *txt, uint16_t c, int p) {   // solo redibuja si cambio algo
  if (!pieForzar && strcmp(txt, pieTxt) == 0 && c == pieCol && p == pieProg) return;
  pieForzar = false;
  strncpy(pieTxt, txt, sizeof(pieTxt) - 1);
  pieTxt[sizeof(pieTxt) - 1] = '\0';
  pieCol = c;
  pieProg = p;
  tft.fillRect(0, PIE_Y, 320, 240 - PIE_Y, colFondo);
  if (txt[0] == '\0') return;
  tft.setTextDatum(TC_DATUM);
  tft.setTextPadding(0);
  tft.setTextColor(c, colFondo);
  const char *barra = strchr(txt, '|');
  if (barra) {   // dos lineas (mensajes de IA)
    char l1[56], l2[56];
    size_t n1 = barra - txt; if (n1 > sizeof(l1) - 1) n1 = sizeof(l1) - 1;
    memcpy(l1, txt, n1); l1[n1] = '\0';
    strncpy(l2, barra + 1, sizeof(l2) - 1); l2[sizeof(l2) - 1] = '\0';
    recortarAncho(l1); recortarAncho(l2);
    tft.drawString(l1, 160, PIE_Y + 2, 2);
    tft.drawString(l2, 160, PIE_Y + 20, 2);
  } else {
    char l[100];
    strncpy(l, txt, sizeof(l) - 1); l[sizeof(l) - 1] = '\0';
    recortarAncho(l);
    tft.drawString(l, 160, PIE_Y + 3, 2);
  }
  tft.setTextDatum(TL_DATUM);
  if (p >= 0) drawBar(60, PIE_Y + 22, 200, 3, p, 100, COL_TURQUESA);
}

// corta el texto hasta que quepa en 312 px (fuente 2)
void recortarAncho(char *t) {
  size_t n = strlen(t);
  while (n > 0 && tft.textWidth(t, 2) > 312) t[--n] = '\0';
}

// =====================================================================
//              MENSAJES DE IA (linea por USB:  IA:texto)
// =====================================================================
void iaRecibir(uint32_t now) {
  int lim = 64;   // no acaparar el loop
  while (Serial.available() > 0 && lim-- > 0) {
    int ch = Serial.read();
    if (ch < 0) break;
    if (ch == '\r') continue;
    if (ch == '\n') {
      rxBuf[rxN] = '\0';
      if (!rxDesborde && rxN > 3 && strncmp(rxBuf, "IA:", 3) == 0) {
        size_t k = 0;
        for (const char *q = rxBuf + 3; *q && k < sizeof(iaTxt) - 1; q++) {
          if (*q >= 32 && *q <= 126) iaTxt[k++] = *q;   // solo ASCII (las fuentes no tienen acentos)
        }
        iaTxt[k] = '\0';
        if (k > 0) {
          iaHay = true;
          iaHasta = now + IA_VIGENTE_MS;
          iaRecibidoMs = now;
          iaHablaDesde = now;             // 3.7: la boquita "habla"
          iniciarGesto(G_SALTITO, now);   // te mira y da un saltito
          tgtX = 0; tgtY = 0; nextLook = now + 2500;
          iaEsperaHasta = 0;
          if (!mute) beepSeq(2, 1200, 40, 60, 200);
          Serial.print(F("# IA recibido: "));
          Serial.println(iaTxt);
        }
      }
      if (!rxDesborde && strncmp(rxBuf, "HORA:", 5) == 0) {          // 3.8: hora de la compu
        uint32_t e = strtoul(rxBuf + 5, nullptr, 10);
        if (e > 1600000000UL) horaBase = e - millis() / 1000;
        const char *coma = strchr(rxBuf + 5, ',');               // 4.0: zona horaria
        if (coma) { long z = atol(coma + 1); if (z > -50400L && z < 50400L) zonaSeg = z; }
      }
      if (!rxDesborde && strncmp(rxBuf, "AVISO:", 6) == 0) {           // 4.0: mensaje corto bajo los ojos
        char t[90]; size_t k = 0;
        for (const char *q = rxBuf + 6; *q && k < sizeof(t) - 1; q++) if (*q >= 32 && *q <= 126) t[k++] = *q;
        t[k] = '\0';
        if (k) mostrarMsg(t, 60000UL, COL_TURQUESA);
      }
      if (!rxDesborde && strcmp(rxBuf, "IA_PENSANDO") == 0) {          // 3.8: pregunta desde la web
        iaEsperaHasta = now + 25000UL;
      }
      if (!rxDesborde && strncmp(rxBuf, "GESTO:", 6) == 0) {
        iaGesto(rxBuf + 6, now);
        Serial.print(F("# gesto: "));
        Serial.println(rxBuf + 6);
      }
      rxN = 0; rxDesborde = false;
    } else if (rxN < sizeof(rxBuf) - 1) {
      rxBuf[rxN++] = (char)ch;
    } else {
      rxDesborde = true;   // linea demasiado larga: se descarta entera
    }
  }
}

bool iaVigente() { return iaHay && !pasado(iaHasta); }

void iaPedir(uint32_t now) {
  Serial.println(F("IA_PEDIR"));
  if (iaHay) iaEsperaHasta = now + IA_ESPERA_MS;   // solo si la compu ya mando algo
}

// progreso de calentamiento de PMS y SCD (0..99), -1 si ya no calienta
int progPMS(uint32_t now, uint32_t &rem) {
  if (pmsSt != SS_CALENTANDO) return -1;
  uint32_t el = now - pmsT0;
  rem = el < PMS_CALENTAR_MS ? PMS_CALENTAR_MS - el : 0;
  int p = (int)(el * 100UL / PMS_CALENTAR_MS);
  return p > 99 ? 99 : p;
}
int progSCD(uint32_t now, uint32_t &rem) {
  if (scdSt != SS_CALENTANDO) return -1;
  uint32_t el = now - scdT0;
  int pT = el >= SCD_CALENTAR_MS ? 100 : (int)(el * 100UL / SCD_CALENTAR_MS);
  int pN = scdN >= SCD_MIN_LECTURAS ? 100 : (int)(scdN * 100UL / SCD_MIN_LECTURAS);
  rem = el < SCD_CALENTAR_MS ? SCD_CALENTAR_MS - el : 0;
  uint32_t remN = scdN < SCD_MIN_LECTURAS ? (uint32_t)(SCD_MIN_LECTURAS - scdN) * 2000UL : 0;
  if (remN > rem) rem = remN;
  int p = pT < pN ? pT : pN;
  return p > 99 ? 99 : p;
}

// =====================================================================
//                              LOGO
// =====================================================================
void showSplash() {
  tft.fillScreen(colFondo);
  tft.setTextDatum(TL_DATUM);
  tft.setTextPadding(0);
#if USAR_FUENTE_GFX
  tft.setFreeFont(&FreeSansBold24pt7b);
  int wAll  = tft.textWidth("NIMBO");
  int wNimb = tft.textWidth("NIMB");
  int h     = tft.fontHeight();
  int x0 = (320 - wAll) / 2;
  int y0 = (240 - h) / 2;
  tft.setTextColor(COL_MARFIL);
  tft.drawString("NIMB", x0, y0);
  tft.setTextColor(COL_LIMA);
  tft.drawString("O", x0 + wNimb, y0);
  tft.setFreeFont(NULL);
#else
  tft.setTextSize(2);
  int wAll  = tft.textWidth("NIMBO", 4);
  int wNimb = tft.textWidth("NIMB", 4);
  int h     = tft.fontHeight(4);
  int x0 = (320 - wAll) / 2;
  int y0 = (240 - h) / 2;
  for (int dx = 0; dx < 2; dx++) {
    tft.setTextColor(COL_MARFIL);
    tft.drawString("NIMB", x0 + dx, y0, 4);
    tft.setTextColor(COL_LIMA);
    tft.drawString("O", x0 + wNimb + dx, y0, 4);
  }
  tft.setTextSize(1);
#endif
}

// =====================================================================
//                              OJOS
// =====================================================================

// ---------------------------------------------------------------------
//  Ojo parametrico: cada cara es un juego de numeros; el ojo dibujado
//  se acerca a ellos un poco en cada cuadro, asi las caras se transforman.
// ---------------------------------------------------------------------

float suave(float t) { if (t <= 0) return 0; if (t >= 1) return 1; return t * t * (3 - 2 * t); }

void colorA(uint16_t c, float &r, float &g, float &b) { r = (c >> 11) & 31; g = (c >> 5) & 63; b = c & 31; }
uint16_t colorDe(float r, float g, float b) {
  return (uint16_t)(((uint16_t)(r + 0.5f) << 11) | ((uint16_t)(g + 0.5f) << 5) | (uint16_t)(b + 0.5f));
}

void objetivoDe(Expr e, Ojo &o) {
  uint16_t c = COL_LIMA;
  o = {26, 38, 0, 0, 0, 0, 0, 0, 0};
  switch (e) {
    case E_HAPPY:     o.w = 28; o.h = 34; o.bot = 0.85f; o.dy = -6; break;
    case E_ATENTO:    o.h = 36; o.top = 0.10f; c = COL_MARFIL; break;   // 2.8: antes h 34, top 0.22 (se veia serio)
    case E_WORRIED:   o.w = 25; o.h = 36; o.top = 0.26f; o.slope = -11; c = COL_CORAL; break;
    case E_ANGRY:     o.h = 36; o.top = 0.30f; o.slope = 17; c = COL_CORAL; break;
    case E_SURPRISED: o.w = 30; o.h = 44; c = COL_MARFIL; break;
    case E_DIZZY:     c = COL_CORAL; break;
    case E_MOLESTO:   o.w = 27; o.h = 30; o.top = 0.40f; o.slope = 20; c = COL_CORAL; break;   // "YA!"
    default: break;    // E_NORMAL: ovalo lima
  }
  colorA(c, o.r, o.g, o.b);
}

void ojoActualizar() {          // un paso de la transformacion (llamar 1 vez por cuadro)
  Ojo t;
  objetivoDe(expr, t);
  // 3.9: en las reacciones (YA!, chueco) los ojos conservan el color del animo (el color = el aire)
  if (expr == E_MOLESTO || expr == E_SURPRISED || expr == E_DIZZY ||
      (expr == E_WORRIED && incomodo && exprAnimo != E_WORRIED)) {   // 4.1: susto y mareo tambien
    Ojo a; objetivoDe(exprAnimo, a); t.r = a.r; t.g = a.g; t.b = a.b;
  }
  if (!ojoListo) { ojo = t; ojoListo = true; return; }
  const float k = (expr == E_SURPRISED) ? 0.25f : 0.10f;   // sorpresa ~0.3 s; el resto ~0.8 s
  ojo.w += (t.w - ojo.w) * k;   ojo.h += (t.h - ojo.h) * k;
  ojo.top += (t.top - ojo.top) * k;   ojo.slope += (t.slope - ojo.slope) * k;
  ojo.bot += (t.bot - ojo.bot) * k;   ojo.dy += (t.dy - ojo.dy) * k;
  const float kc = 0.025f;   // 4.1: el color cambia despacio (~3 s): el amarillo se nota mas
  ojo.r += (t.r - ojo.r) * kc;  ojo.g += (t.g - ojo.g) * kc;  ojo.b += (t.b - ojo.b) * kc;
}

void drawEye(int cx, int cy, int side) {
  const uint16_t ec = colorDe(ojo.r, ojo.g, ojo.b);
  const uint16_t bg = colFondo;          // los parpados son del color del fondo
  bool cansable = (expr != E_SURPRISED && expr != E_DIZZY && expr != E_HAPPY);
  float abre = openAmt * (cansable ? cansancio : 1.0f) * gAbre[side] * (1.0f - 0.94f * dormirAmt)
               * (1.0f - 0.25f * aburridoAmt)                                     // 3.9: aburrido = parpados pesados
               * ((cansable && esNoche()) ? 0.9f : 1.0f);                         // 4.0: de noche, un poco mas cerrados
  float sq = 1.0f + 0.15f * (1.0f - openAmt);   // al parpadear se aplasta y se ensancha

  int ox, oy;
  if (expr == E_DIZZY) {
    ox = (int)(lookX * 16) + random(-2, 3);
    oy = (int)(lookY * 14) + random(-2, 3);
  } else {
    ox = (int)(lookX * 14);
    oy = (int)(lookY * 9);
  }
  cx += ox + offX;
  cy += oy + offY + (int)ojo.dy + (int)gDy[side];

  float hFull = ojo.h * escY * gEsc[side];
  int w = (int)(ojo.w * escX * sq * gEsc[side]);
  int h = (int)(hFull * abre);
  if (h < 2) h = 2;
  spr.fillEllipse(cx, cy, w, h, ec);

  // parpado de arriba (con inclinacion: el lado interior es el que mira a la nariz)
  if (ojo.top > 0.01f || fabsf(ojo.slope) > 0.5f) {
    float base = cy - h + ojo.top * 2.0f * h;
    float s = ojo.slope * escY;
    float yIn = base + s, yOut = base - s;
    int xL = cx - w - 4, xR = cx + w + 4, yT = cy - h - 40;
    int yL = (int)(side == 0 ? yOut : yIn);   // ojo izquierdo: su interior es la derecha
    int yR = (int)(side == 0 ? yIn : yOut);
    spr.fillTriangle(xL, yT, xR, yT, xR, yR, bg);
    spr.fillTriangle(xL, yT, xR, yR, xL, yL, bg);
  }
  // parpado de abajo curvo (ojos sonrientes)
  if (ojo.bot > 0.01f) {
    int my = cy + (int)(h * (2.0f - 1.4f * ojo.bot));
    spr.fillEllipse(cx, my, (int)(w * 1.35f), h, bg);
  }
}



void iniciarSuspiro(uint32_t now) {
  suspiroStart = now;
  if (!mute) beepSeq(2, 880, 110, 40, -260);
}

void iniciarBostezo(uint32_t now) {
  bostezoStart = now;
  bocaPoner(BOCA_BOSTEZO, BOSTEZO_MS);                       // 3.9
  bostezoCool = now + BOSTEZO_CADA_MS;
  blinking = false;
  openAmt = 1.0;
  if (!mute) beepSeq(3, 520, 140, 10, -90);
}

void setExpr(Expr e, bool sound) {
  expr = e;
  if (sound && !mute && e < E_COUNT) {
    const uint16_t f[E_COUNT] = {600, 1200, 400, 300, 200};
    beep(f[e], 90);
  }
}

// ---------- compania (3.0) ----------
bool puedeDormir() { return estado >= ST_ATENTO && estado <= ST_FELIZ; }   // nunca en Preocupado/Alerta

void despertar(uint32_t now) {
  if (!durmiendo) return;
  durmiendo = false;
  nextBlink = now + 400;
  evento("despierta");
}

// cara por un rato (caricia, saludo, toques); cancela reacciones
void caraTemporal(Expr e, uint32_t ms, uint32_t now) {
  reactUntil = 0;
  setExpr(e, false);
  uint32_t hasta = now + ms;
  if (manualUntil == 0 || (int32_t)(hasta - manualUntil) > 0) manualUntil = hasta;
  popStart = now;
  popAmp = 0.07f;
}

// animaciones de gusto (3.1): se suman a la cara feliz para que no se vea siempre igual
enum { AN_NADA, AN_MECE, AN_ACURRUCA, AN_BRINCA, AN_DERRITE, AN_COSQUILLAS, AN_BAILE, AN_ESTIRA };
void animFeliz(uint8_t tipo, uint32_t dur, uint32_t now) {
  caraTemporal(E_HAPPY, dur, now);
  bocaPoner(tipo == AN_COSQUILLAS || tipo == AN_BRINCA || tipo == AN_BAILE ? BOCA_RISA : BOCA_SONRISA, dur);   // 3.9
  animTipo = tipo; animStart = now; animDur = dur;
  tgtX = 0; tgtY = 0; nextLook = now + dur;
}

// 4.0: estirarse (buenos dias, pausa): ojos altos hacia arriba, tiemblan un poquito y se relajan
void estirarse(uint32_t now) {
  caraTemporal(exprAnimo, 2600, now);
  animTipo = AN_ESTIRA; animStart = now; animDur = 2600;
  tgtX = 0; tgtY = -0.3f; nextLook = now + 2600;
  bocaPoner(BOCA_BOSTEZO, 2600);
  if (!mute) beepSeq(4, 500, 120, 30, 110);
}

// 4.0: resumen del dia
void fmtDur(char *b, size_t n, uint16_t m) {
  if (m < 60) snprintf(b, n, "%u min", m);
  else if (m % 60 == 0) snprintf(b, n, "%u h", m / 60);
  else snprintf(b, n, "%u h %u min", m / 60, m % 60);
}
void mostrarResumen(uint32_t now) {
  char b[90], d1[20], d2[20];
  int mejor = 4;
  for (int i = 0; i < 5; i++) if (minEstado[i] > minEstado[mejor]) mejor = i;
  uint16_t total = 0; for (int i = 0; i < 5; i++) total += minEstado[i];
  if (total == 0) { mostrarMsg("Todavia no junto datos de hoy", 6000, COL_TURQUESA); return; }
  uint16_t malo = minEstado[ST_ALERTA] + minEstado[ST_PREOCUPADO];
  fmtDur(d1, sizeof(d1), minEstado[mejor]);
  if (malo > 0) { fmtDur(d2, sizeof(d2), malo); snprintf(b, sizeof(b), "%s: %s %s|Aire malo %s, ventilaste %u", horaOk() ? "Hoy" : "Desde que prendi", EST_TXT[mejor], d1, d2, ventilacionesHoy); }
  else snprintf(b, sizeof(b), "%s: %s %s|Ventilaste %u %s", horaOk() ? "Hoy" : "Desde que prendi", EST_TXT[mejor], d1, ventilacionesHoy, ventilacionesHoy == 1 ? "vez" : "veces");
  mostrarMsg(b, 15000, COL_TURQUESA);
  if (reactUntil == 0 && manualUntil == 0) iniciarGesto(G_LADEO, now);
  evento(b);
}

// alguien le hizo caso. Si estaba dormido o hacia rato que nadie lo pelaba, te saluda
// (solo si saludar = true). Devuelve true si saludo.
bool interaccion(uint32_t now, bool saludar) {
  bool tocaSaludo = durmiendo || (now - ultimaInteraccion > SALUDO_MS && now > SALUDO_MS);
  despertar(now);
  ultimaInteraccion = now;
  if (!tocaSaludo || !saludar) return false;
  animFeliz(AN_BRINCA, 2200, now);
  popAmp = 0.10f;
  if (!mute) beepSeq(3, 900, 60, 40, 300);
  evento("saludo");
  return true;
}

void caricia(uint32_t now) {   // boton A en OJOS
  interaccion(now, false);
  caricias = (now - ultCaricia < 15000UL && caricias < 200) ? caricias + 1 : 1;
  ultCaricia = now;
  uint8_t tipo;
  if (caricias % 4 == 0) tipo = AN_DERRITE;                     // cada 4 seguidas: se derrite
  else { do { tipo = AN_MECE + random(3); } while (tipo == animPrev); }   // nunca la misma dos veces
  animPrev = tipo;
  switch (tipo) {
    case AN_MECE:     animFeliz(tipo, 2600, now); if (!mute) beepSeq(5, 320, 60, 40, -14); break;   // ronroneo
    case AN_ACURRUCA: animFeliz(tipo, 2400, now); if (!mute) beepSeq(2, 760, 140, 40, -220); break;  // "aaah"
    case AN_BRINCA:   animFeliz(tipo, 1700, now); if (!mute) beepSeq(3, 1000, 40, 30, 320); break;  // yupi
    default:          animFeliz(AN_DERRITE, 5000, now); if (!mute) beepSeq(4, 660, 90, 50, 110); break;
  }
  evento("caricia");
}

// golpe nuevo (cada toque se nota al momento con un brinquito)
void toqueNuevo(uint32_t now) {
  if (serieToques < 50) serieToques++;
  serieUlt = now;
  if (noiseCooldown < now + 500) noiseCooldown = now + 500;   // el ruido del toque no lo asusta
  if (durmiendo || reactUntil != 0) return;
  popStart = now; popAmp = 0.10f;
  tapFlash = now;                            // 3.6: brinca y se aplasta un poquito
  tgtX = 0; tgtY = 0; nextLook = now + 1500;
  if (!mute) beep(1500, 25);
  if (serieToques >= YA_TOQUES) {            // ya, no esperes a que termine la serie
    serieToques = 0;
    interaccion(now, false);
    startReaction(E_MOLESTO, 3500);          // 3.7: mas largo
    tapCool = now + 4000;                    // un rato sin hacer caso a los toques
    evento("toques: YA!");
  }
}

// termino la serie de toques: reacciona segun cuantos fueron
void serieTerminada(uint32_t now) {
  uint8_t n = serieToques;
  serieToques = 0;
  if (n == 0) return;
  if (interaccion(now, true)) return;        // estaba dormido o te extranaba: el saludo gana
  if (n == 1) {                              // "si?": te mira, ladea la cabeza, pregunta
    iniciarToque(now); iniciarGesto(G_LADEO, now);
    bocaPoner(BOCA_PREGUNTA, 2000);
    if (!mute) beepSeq(2, 900, 60, 30, 500);
    evento("toque: si?");
  } else { animFeliz(AN_COSQUILLAS, 3500, now); if (!mute) beepSeq(5, 1300, 30, 25, 160); evento("toques: risa"); }   // 4.2: 2 o 3 = risa
}

void startReaction(Expr e, uint32_t duration) {
  ultimaInteraccion = millis();
  if (durmiendo) despertar(millis());   // un susto o una sacudida lo despiertan
  if (reactUntil == 0) exprBeforeReact = (manualUntil != 0) ? exprAnimo : expr;
  manualUntil = 0;
  animTipo = AN_NADA;
  expr = e;
  reactStartMs = millis();
  reactUntil = reactStartMs + duration;
  blinking = false;
  openAmt = 1.0;
  popStart = reactStartMs;                    // rebote al reaccionar
  popAmp = (e == E_SURPRISED) ? 0.10f : 0.04f;
  if (e == E_MOLESTO) bocaPoner(BOCA_GRR, duration);        // 3.9
  else if (e == E_SURPRISED) bocaPoner(BOCA_O, duration);
  else if (e == E_DIZZY) bocaPoner(BOCA_ONDA, duration);
  if (!mute) {
    if (e == E_DIZZY) beep(250, 200);
    else if (e == E_SURPRISED) beep(1800, 120);
    else if (e == E_MOLESTO) beepSeq(3, 300, 90, 30, -40);   // grrr
  }
}

void readSensorsForReactions() {
  uint32_t now = millis();
  if (now - lastSensorRead < 40) return;
  lastSensorRead = now;
  bool botonApretado = false;
  for (int i = 0; i < NBTN; i++) if (btnState[i]) botonApretado = true;
  bool ignorar = botonApretado || (now - lastInputMs < INPUT_IGNORE_MS) ||   // 4.1: botones de arriba no son toques
                 (acostadoDesde && now - acostadoDesde < 3500);             // ni el golpe al acostarlo
  bool buzzerReciente = buzzing || (now - buzzEndMs < 250);

  if (accelOk) {
    acelRapido(now);
    // promedio de las lecturas rapidas de esta ventana: los golpes cortos casi no lo mueven
    float x = acN ? acSX / acN : acX, y = acN ? acSY / acN : acY, z = acN ? acSZ / acN : acZ;
    acSX = acSY = acSZ = 0; acN = 0;

    float mag = sqrtf(x * x + y * y + z * z);
    if (mag < FREEFALL_G) {
      if (ffCount < 255) ffCount++;
      if (ffCount == FREEFALL_SAMPLES && !ignorar) {
        startReaction(E_SURPRISED, NOISE_DURATION);
        evento("caida libre");
      }
    } else {
      ffCount = 0;
    }

    // --- inclinacion: gravedad filtrada contra la posicion "derecha" fija ---
    if (!gOk) { gx = x; gy = y; gz = z; gOk = true; }
    gx += (x - gx) * 0.25f; gy += (y - gy) * 0.25f; gz += (z - gz) * 0.25f;
    angulos(gx, gy, gz, tiltDeg, frenteDeg);
    bool vol = (gx * TILT_REF_X + gy * TILT_REF_Y + gz * TILT_REF_Z) < -0.5f;   // boca abajo
    if (vol && !volteado && !ignorar) startReaction(E_SURPRISED, NOISE_DURATION);
    volteado = vol;

    float maxAng = fabsf(tiltDeg) > fabsf(frenteDeg) ? fabsf(tiltDeg) : fabsf(frenteDeg);
    bool incl = volteado || maxAng > 12.0f;
    if (incl && !inclinado) interaccion(now, false);   // moverlo cuenta como hacerle caso
    inclinado = incl;
    // 3.9: acostado boca arriba (pantalla al techo; prueba 26-nov: atras = -77/-80) -> se duerme
    bool atras = !volteado && frenteDeg < -55.0f;
    if (atras) { if (!acostadoDesde) acostadoDesde = now; }
    else if (frenteDeg > -35.0f) acostadoDesde = 0;
    bool acost = acostadoDesde && now - acostadoDesde > 1500;
    if (acost && !acostado) {
      acostado = true;
      if (!durmiendo) { durmiendo = true; dormidoPorAcostar = true; iniciarBostezo(now); evento("acostado: se duerme"); }
    } else if (!acost && acostado) {
      acostado = false;
      if (durmiendo && dormidoPorAcostar) { interaccion(now, true); evento("levantado: despierta"); }
      dormidoPorAcostar = false;
    }
    float angChueco = (frenteDeg < -45.0f) ? fabsf(tiltDeg) : maxAng;   // echado hacia atras no es "chueco"
    if (volteado || angChueco > CHUECO_ON) chueco = true;
    else if (angChueco < CHUECO_OFF) chueco = false;
    if (chueco) { if (!chuecoDesde) chuecoDesde = now; }
    else chuecoDesde = 0;
    bool inc = chuecoDesde && now - chuecoDesde >= CHUECO_MS;
    if (incomodo && !chueco) { iniciarSuspiro(now); evento("enderezado: alivio"); }
    incomodo = inc;

    bool hayClick = clickLeer();          // se lee siempre para borrarlo, aunque se ignore
    if (hayClick) toquesHw++;
    if (acDeltaMax > golpeMaxDbg) golpeMaxDbg = acDeltaMax;
    // --- sacudida (promedios de 40 ms, sostenida) y toquecito (golpe entre lecturas rapidas) ---
    if (havePrev) {
      float delta = fabs(x - pax) + fabs(y - pay) + fabs(z - paz);
      energia += (delta - energia) * 0.25f;   // sube con movimiento continuo, baja rapido tras un golpe
    }
    float thrS = SHAKE_THR[ajSens] * 0.6f;
    bool mareado = reactUntil != 0 && expr == E_DIZZY;
    if (havePrev && !ignorar) {
      float golpe = acDeltaMax;
      bool sacudiendo = energia > thrS * 0.7f || mareado;
      bool hay = clickHw ? hayClick : (golpe > tapThr);    // 3.4: el acelerometro guarda el toque
      if (ajSens > 0 && hay && !buzzerReciente && !sacudiendo && pasado(tapCool)) {
        if (now - ultGolpe > TAP_REBOTE_MS) toqueNuevo(now);   // golpe nuevo (no el rebote del anterior)
        ultGolpe = now;
      }
      if (sacudiendo) serieToques = 0;
#if DEBUG_ACCEL
      if (golpe > 0.2) { Serial.print(F("# golpe ")); Serial.println(golpe, 2); }
#endif
      if (energia > thrS) {
        if (mareado) {                                      // lo sigues sacudiendo: sigue mareado
          if ((int32_t)(now + 1200 - reactUntil) > 0) reactUntil = now + 1200;
        } else if ((shakeMs += 40) >= SHAKE_SOSTENIDO_MS) {
          shakeMs = 0;
          serieToques = 0;
          startReaction(E_DIZZY, SHAKE_DURATION);
          evento("sacudida: mareado");
        }
      } else if (energia < thrS * 0.7f) shakeMs = 0;
    }
    acDeltaMax = 0;
    pax = x; pay = y; paz = z;
    havePrev = true;
    if (serieToques && now - serieUlt >= SERIE_FIN_MS && reactUntil == 0) serieTerminada(now);
  }

  // --- microfono: sorpresa y "te escucha", ambos contra el ruido de fondo ---
  medirMic(buzzerReciente);
  int thr = umbralSorpresa();
#if DEPURAR
  {
    static uint32_t proxDbg = 0;
    if (pasado(proxDbg)) {
      proxDbg = now + 200;
      Serial.print(F("dbg,")); Serial.print(now);
      Serial.print(F(",mic,")); Serial.print(micAhora);
      Serial.print(F(",fondo,")); Serial.print((int)ruidoBase);
      Serial.print(F(",uvoz,")); Serial.print((int)umbralVoz());
      Serial.print(F(",susto,")); Serial.print(thr);
      Serial.print(F(",vozc,")); Serial.print(vozCont);
      Serial.print(F(",escucha,")); Serial.print(pasado(escuchaHasta) ? 0 : 1);
      Serial.print(F(",golpe,")); Serial.print(golpeMaxDbg, 2);
      Serial.print(F(",energia,")); Serial.print(energia, 2);
      Serial.print(F(",toquesHw,")); Serial.print(toquesHw);
      Serial.print(F(",serie,")); Serial.print(serieToques);
      Serial.print(F(",co2,")); Serial.print(scdCO2);
      Serial.print(F(",co2prom,")); Serial.print(vCO2.ema.ok ? (int)vCO2.ema.v : -1);
      Serial.print(F(",co2usa,")); Serial.println(isnan(co2Efectivo) ? -1 : (int)co2Efectivo);
      golpeMaxDbg = 0;
    }
  }
#endif
  bool silencioCama = acostado && durmiendo;     // 4.1: acostado y dormido: la voz y los ruidos no lo despiertan
  if (thr > 0 && !ignorar && !silencioCama && !buzzerReciente && now >= noiseCooldown && !(reactUntil != 0 && expr == E_DIZZY)) {
    if (micAhora > thr) {
      startReaction(E_SURPRISED, NOISE_DURATION);
      noiseCooldown = now + SORPRESA_COOL_MS;
    }
  }
  if (silencioCama) vozCont = 0;                 // lo que oyo dormido no cuenta al despertar
  if (!buzzerReciente) {
    bool voz = vozAhora && (thr == 0 || micAhora < thr);
    vozCont += voz ? 2 : -1;
    if (vozCont < 0) vozCont = 0;
    if (vozCont > 30) vozCont = 30;
    if (vozCont >= 8 && ajSens > 0 && !silencioCama) {
      if (pasado(escuchaHasta)) {                         // empieza a escucharte
        escuchaDesde = now;
        if (!interaccion(now, true) && reactUntil == 0) {  // (si no te saludo) levanta la mirada y ladea la cabeza
          popStart = now; popAmp = 0.15f;
          if (!mute) beepSeq(2, 1000, 40, 30, 300);         // "mm?" (3.6)
          if (manualUntil == 0) iniciarGesto(G_LADEO, now);
          evento("te escucha");
        }
      } else ultimaInteraccion = now;
      escuchaHasta = now + ESCUCHA_MS;
    }
  }
}

void iniciarToque(uint32_t now) {   // "si?": te mira, doble parpadeo y un rebotito
  tgtX = 0; tgtY = 0; nextLook = now + 4000;   // 3.7: te mira mas rato
  blinking = true; blinkStart = now; dobleParpadeo = true;
  popStart = now; popAmp = 0.05f;
}

void iaGesto(const char *g, uint32_t now) {   // gesto elegido por la IA
  if (reactUntil != 0) return;
  if (!strcmp(g, "guino"))          iniciarGesto(G_GUINO, now);
  else if (!strcmp(g, "saltito"))   iniciarGesto(G_SALTITO, now);
  else if (!strcmp(g, "ladeo"))     iniciarGesto(G_LADEO, now);
  else if (!strcmp(g, "alrededor")) iniciarGesto(G_ALREDEDOR, now);
  else if (!strcmp(g, "sorpresa"))  startReaction(E_SURPRISED, NOISE_DURATION);
  else if (!strcmp(g, "feliz"))     { setExpr(E_HAPPY, false); manualUntil = now + 6000; }
  else if (!strcmp(g, "estirar"))   { despertar(now); estirarse(now); }   // 4.0: pausa
}

void iniciarGesto(Gesto g, uint32_t now) {
  gesto = g;
  gestoStart = now;
  gestoLado = random(2);
  if (g == G_ALREDEDOR) nextLook = now + GESTO_MS[G_ALREDEDOR] + 300;
}

// calcula el efecto del gesto en curso (escala, altura y apertura de cada ojo)
void gestoActualizar(uint32_t now) {
  gEsc[0] = gEsc[1] = 1; gDy[0] = gDy[1] = 0; gAbre[0] = gAbre[1] = 1;
  if (gesto == G_NADA) return;
  uint32_t dur = GESTO_MS[gesto], t = now - gestoStart;
  if (t >= dur) { gesto = G_NADA; return; }
  float u = t / (float)dur;
  switch (gesto) {
    case G_GUINO:      // un solo ojo se cierra y abre
      gAbre[gestoLado] = fabsf(cosf(PI * u));
      break;
    case G_ALREDEDOR: {   // mira a un lado, al otro y regresa
      float d = gestoLado ? 1.0f : -1.0f;
      if (u < 0.33f)      tgtX = 0.95f * d;
      else if (u < 0.70f) tgtX = -0.95f * d;
      else                tgtX = 0;
      tgtY = -0.15f;
      break;
    }
    case G_LADEO: {       // "cabeza ladeada": un ojo mas grande y mas alto
      float a = (u < 0.25f) ? suave(u / 0.25f) : (u > 0.75f ? 1.0f - suave((u - 0.75f) / 0.25f) : 1.0f);
      gEsc[gestoLado] = 1.0f + 0.12f * a;
      gDy[gestoLado] = -5.0f * a;
      gDy[1 - gestoLado] = 3.0f * a;
      break;
    }
    case G_SALTITO: {     // dos brinquitos
      float k = sinf(PI * fmodf(u * 2.0f, 1.0f));
      gDy[0] = gDy[1] = -7.0f * k * (u < 0.5f ? 1.0f : 0.6f);
      break;
    }
    default: break;
  }
}

void updateEyes() {
  uint32_t now = millis();
  readSensorsForReactions();
  ojoActualizar();

  if (reactUntil != 0 && now >= reactUntil) {
    reactUntil = 0;
    expr = exprBeforeReact;
    nextBlink = now + 600;
    nextLook = now + 400;
  }
  if (manualUntil != 0 && pasado(manualUntil)) manualUntil = 0;
  Expr objetivo = incomodo ? E_WORRIED : exprAnimo;   // chueco mucho rato: preocupado
  if (reactUntil == 0 && manualUntil == 0 && expr != objetivo) {
    expr = objetivo;            // la forma se transforma sola (ojoActualizar)
    popStart = now;             // + un rebote suave
    popAmp = 0.04f;
  }

  // --- dormir: 20 min sin hacerle caso (nunca con aire Preocupado o Alerta) ---
  bool pensandoIA = iaEsperaHasta && !pasado(iaEsperaHasta);
  if (!durmiendo && puedeDormir() && reactUntil == 0 && manualUntil == 0 && !pensandoIA &&
      now - ultimaInteraccion > (esNoche() ? 300000UL : SUENO_MS)) {   // 4.0: de noche a los 5 min
    durmiendo = true;
    iniciarBostezo(now);
    evento("se duerme");
  }
  if (durmiendo && !puedeDormir() && !(acostado && estado != ST_ALERTA)) despertar(now);   // el aire se puso feo: despierta solo
  // 3.9: aburrido (8 min sin hacerle caso): parpados pesados, zzz y bostezos de vez en cuando
  aburrido = !durmiendo && puedeDormir() && now - ultimaInteraccion > (esNoche() ? 120000UL : ABURRIDO_MS);
  // 4.0: rutinas del dia (solo con hora de la compu)
  if (horaOk() && !durmiendo && reactUntil == 0 && manualUntil == 0) {
    uint32_t dia = diaLocal();
    if (esManana() && diaBuenosDias != dia && now - ultimaInteraccion < 60000UL) {   // te ve en la manana
      diaBuenosDias = dia;
      estirarse(now);
      mostrarMsg("Buenos dias! Hoy cuido tu aire", 8000, COL_LIMA);
      evento("buenos dias");
    } else if (esNoche() && diaNoche != dia && horaLocal() >= 22) {
      diaNoche = dia;
      iniciarBostezo(now);
      mostrarMsg("Ya es de noche... que sueno", 8000, COL_TURQUESA);
      evento("buenas noches");
    } else if (horaLocal() == 20 && diaResumen != dia && (minEstado[0] + minEstado[1] + minEstado[2] + minEstado[3] + minEstado[4]) > 30) {
      diaResumen = dia;
      mostrarResumen(now);
    }
  }
  aburridoAmt += ((aburrido ? 1.0f : 0.0f) - aburridoAmt) * 0.03f;
  if (aburrido && reactUntil == 0 && manualUntil == 0 && pasado(proxBostezoAburrido)) {
    if (proxBostezoAburrido) iniciarBostezo(now);
    proxBostezoAburrido = now + random(120000, 200000);
  }
  if (!aburrido) proxBostezoAburrido = 0;
  {
    bool bost = bostezoStart && now - bostezoStart < BOSTEZO_MS;
    float objD = (durmiendo && !bost) ? 1.0f : 0.0f;
    dormirAmt += (objD - dormirAmt) * (objD > dormirAmt ? 0.012f : 0.15f);   // cierra en ~3 s, abre rapido
  }

  // --- el aire en la cara (3.1) ---
  if (tosPend && reactUntil == 0) {          // pico de particulas: tose (y despierta, es importante)
    tosPend = false;
    despertar(now);
    caraTemporal(E_WORRIED, 2000, now);
    animTipo = AN_NADA;
    tosStart = now;
    if (!mute) beepSeq(3, 240, 90, 360, 0);
    evento("tos (pico de particulas)");
  }
  if (empeoroPend && reactUntil == 0 && manualUntil == 0) {   // empeoro un nivel: se sobresalta un momento
    empeoroPend = false;
    if (!durmiendo) {
      uint32_t u = ultimaInteraccion;
      startReaction(E_SURPRISED, 700);
      ultimaInteraccion = u;                 // el aire no cuenta como que le hicieron caso
      if (!mute) beepSeq(2, 700, 90, 40, -250);
    }
  }

  // --- eventos de personalidad (nunca durante una reaccion o cara manual, ni dormido) ---
  if (reactUntil == 0 && manualUntil == 0 && !durmiendo) {
    if (estado <= ST_FELIZ && estadoAnim <= ST_FELIZ && estado > estadoAnim) iniciarSuspiro(now);   // el aire mejoro
    if (!pasado(evVentanaHasta) && evVentanaHasta != ventanaVista) ventanaVista = evVentanaHasta;
    if (bailePend) {                                   // 4.0: abriste la ventana: bailecito
      bailePend = false;
      animFeliz(AN_BAILE, 3600, now);
      if (!mute) beepSeq(6, 660, 90, 40, 110);
      mostrarMsg("Aire fresco! Gracias", 7000, COL_LIMA);
      evento("ventilaste: baile");
    }
    bool alto = !isnan(co2Efectivo) && co2Efectivo >= 1000;
    if (alto && !co2Alto && pasado(bostezoCool)) iniciarBostezo(now);
    if (alto) co2Alto = true;
    else if (isnan(co2Efectivo) || co2Efectivo < 950) co2Alto = false;

    // gesto suelto cada 15-40 s (no en Alerta ni bostezando/suspirando)
    bool ocupado = (bostezoStart && now - bostezoStart < BOSTEZO_MS) || (suspiroStart && now - suspiroStart < SUSPIRO_MS);
    if (gesto == G_NADA && !ocupado && pasado(proxGesto) && estado != ST_ALERTA && pasado(escuchaHasta)) {
      bool contento = (estado == ST_FELIZ || estado == ST_TRANQUILO);
      int r = random(contento ? 4 : 3);
      iniciarGesto(r == 0 ? G_GUINO : r == 1 ? G_ALREDEDOR : r == 2 ? G_LADEO : G_SALTITO, now);
      proxGesto = now + random(15000, 40000);
    }
  } else {
    gesto = G_NADA;
  }
  gestoActualizar(now);
  estadoAnim = estado;

  // --- inclinacion: los ojos resbalan hacia el lado bajo y se quedan nivelados ---
  {
    float t = volteado ? 0 : tiltDeg, f = volteado ? 0 : frenteDeg;
    t = fabsf(t) < TILT_MUERTO ? 0 : (t > 0 ? t - TILT_MUERTO : t + TILT_MUERTO);
    f = fabsf(f) < TILT_MUERTO ? 0 : (f > 0 ? f - TILT_MUERTO : f + TILT_MUERTO);
    t = constrain(t, -30.0f, 30.0f);
    f = constrain(f, -30.0f, 30.0f);
    tiltS += (t - tiltS) * 0.18f;
    frenteS += (f - frenteS) * 0.18f;
    float nivel = 60.0f * sinf(tiltS * PI / 180.0f);   // un ojo sube y el otro baja: siguen el horizonte
    gDy[0] += nivel;
    gDy[1] -= nivel;
  }

  // --- cansancio progresivo: CO2 promedio 800 -> 1400 ppm = apertura 100 % -> 85 % ---
  float objetivoC = 1.0f;
  if (vCO2.ema.ok) {
    float x = ((isnan(co2Efectivo) ? vCO2.ema.v : co2Efectivo) - 800.0f) / 600.0f;   // 2.8: 800 -> 1400 ppm
    if (x < 0) x = 0;
    if (x > 1) x = 1;
    objetivoC = 1.0f - 0.15f * x;
  }
  cansancio += (objetivoC - cansancio) * 0.02f;

  // --- escala y desplazamiento de este cuadro ---
  float br = sinf(now / (dormirAmt > 0.5f ? 1400.0f : 700.0f));   // respiracion (~4.4 s; dormido ~8.8 s)
  escX = 1.0f;
  escY = 1.0f + 0.02f * br;
  offX = 0;
  offY = (int)((dormirAmt > 0.5f ? 3.0f : 2.0f) * br);
  if (now - popStart < 500) {                         // rebote al cambiar de cara
    float k = 1.0f + popAmp * sinf(PI * (now - popStart) / 500.0f);
    escX *= k; escY *= k;
  }
  if (suspiroStart && now - suspiroStart < SUSPIRO_MS) {   // suspiro: crecen y miran un poco arriba
    float t = (now - suspiroStart) / (float)SUSPIRO_MS;
    float a = (t < 0.35f) ? suave(t / 0.35f) : 1.0f - suave((t - 0.35f) / 0.65f);
    escX *= 1.0f + 0.08f * a;
    escY *= 1.0f + 0.15f * a;
    offY -= (int)(4 * a);
  }
  bool bostezando = bostezoStart && now - bostezoStart < BOSTEZO_MS;
  if (bostezando) {                                   // bostezo: se estiran, se cierran, se abren
    float t = (now - bostezoStart) / (float)BOSTEZO_MS, sx, sy;
    if (t < 0.32f)      { float a = suave(t / 0.32f);           sy = 1.0f + 0.25f * a;  sx = 1.0f - 0.08f * a; }
    else if (t < 0.68f) { float a = suave((t - 0.32f) / 0.36f); sy = 1.25f - 1.10f * a; sx = 0.92f + 0.08f * a; }
    else                { float a = suave((t - 0.68f) / 0.32f); sy = 0.15f + 0.85f * a; sx = 1.0f; }
    escX *= sx; escY *= sy;
  }
  if (estado == ST_ALERTA && reactUntil == 0 && (now % 5000UL) < 1000UL) offX += random(-2, 3);   // temblor
  offX += (int)constrain(tiltS * 1.3f, -38.0f, 38.0f);   // la cara se va hacia el lado bajo
  offY += (int)constrain(frenteS * 0.5f, -12.0f, 12.0f);

  // --- animaciones de gusto (caricias, saludo, cosquillas) ---
  if (animTipo && now - animStart < animDur && manualUntil != 0) {
    float t = (now - animStart) / (float)animDur;
    float env = suave(t / 0.15f) * (1.0f - suave((t - 0.8f) / 0.2f));   // entra y sale suave
    float seg = (now - animStart) / 1000.0f;
    switch (animTipo) {
      case AN_MECE: {          // se mece de lado a lado, como cuando lo acaricias
        float k = sinf(seg * 2 * PI * 0.9f);
        offX += (int)(9 * k * env); gDy[0] += 5 * k * env; gDy[1] -= 5 * k * env;
        break;
      }
      case AN_ACURRUCA:        // se hunde y se aplasta un poquito
        offY += (int)(10 * env); escX *= 1 + 0.07f * env; escY *= 1 - 0.12f * env;
        break;
      case AN_BRINCA: {        // tres brincos de gusto
        float k = fabsf(sinf(t * PI * 3));
        offY -= (int)(11 * k * (1 - t)); escY *= 1 + 0.06f * k;
        break;
      }
      case AN_DERRITE:         // se derrite de gusto
        offY += (int)(14 * env); escX *= 1 + 0.14f * env; escY *= 1 - 0.25f * env;
        offX += (int)(3 * sinf(seg * 2 * PI * 0.5f) * env);
        break;
      case AN_BAILE: {         // 4.0: bailecito: de lado a lado con brinquitos
        float k = sinf(seg * 2 * PI * 1.4f);
        offX += (int)(14 * k * env);
        offY -= (int)(9 * fabsf(sinf(seg * 2 * PI * 2.8f)) * env);
        gDy[0] += 6 * k * env; gDy[1] -= 6 * k * env;
        break;
      }
      case AN_ESTIRA: {        // 4.0: se estira hacia arriba, tiembla y se relaja
        float a = (t < 0.45f) ? suave(t / 0.45f) : 1.0f - suave((t - 0.6f) / 0.4f);
        escY *= 1.0f + 0.30f * a; escX *= 1.0f - 0.12f * a;
        offY -= (int)(10 * a);
        if (t > 0.35f && t < 0.6f) offX += (int)(2 * sinf(seg * 2 * PI * 9));
        break;
      }
      case AN_COSQUILLAS: {    // se rie de lado a lado (4.1: mas lento y suave)
        float k = sinf(seg * 2 * PI * 3);
        offX += (int)(6 * k * env); offY -= (int)(4 * fabsf(k) * env);
        escY *= 1 - 0.05f * fabsf(k) * env;
        break;
      }
      default: break;
    }
  } else animTipo = AN_NADA;

  // --- tos: tres apretones con brinquito ---
  if (tosStart && now - tosStart < 1350) {
    uint32_t t = (now - tosStart) % 450;
    float k = t < 250 ? sinf(PI * t / 250.0f) : 0;
    escY *= 1 - 0.35f * k; escX *= 1 + 0.08f * k; offY += (int)(6 * k);
  } else tosStart = 0;

  // --- brinquito de cada toque (3.6) ---
  if (tapFlash && now - tapFlash < 380) {      // 3.7: un poco mas largo
    float k = sinf(PI * (now - tapFlash) / 380.0f);
    offY -= (int)(12 * k); escX *= 1 + 0.08f * k; escY *= 1 - 0.08f * k;
  } else tapFlash = 0;

  // --- "YA!": sacude la cabeza, entrecierra los ojos y te mira ---
  if (reactUntil != 0 && expr == E_MOLESTO) {
    uint32_t t = now - reactStartMs;
    if (t < 1500) offX += (int)(8.0f * (1.0f - t / 1500.0f) * sinf(t / 20.0f));
    lookX += (0 - lookX) * 0.3f;
    lookY += (0.1f - lookY) * 0.3f;
    lookVX = lookVY = 0;
    return;
  }

  if (reactUntil != 0 && expr == E_DIZZY) {
    float amp = (now - reactStartMs) / 500.0f;               // el giro crece en 0.5 s...
    if (amp > 1) amp = 1;
    if (reactUntil - now < 500) amp *= (reactUntil - now) / 500.0f;   // ...y se apaga en el ultimo 0.5 s
    float t = now / 90.0;
    lookX += (amp * cos(t) - lookX) * 0.35f;
    lookY += (amp * sin(t) - lookY) * 0.35f;
    lookVX = lookVY = 0;
    return;
  }
  if (reactUntil != 0 && expr == E_SURPRISED) {
    lookX += (0 - lookX) * 0.4;
    lookY += (0 - lookY) * 0.4;
    lookVX = lookVY = 0;
    return;
  }

  // --- mirada: destinos al azar + micro-movimientos, con resorte (se pasa un poco y regresa) ---
  if (now >= nextLook) {
    tgtX = random(-10, 11) / 10.0;
    tgtY = random(-6, 7) / 10.0;
    nextLook = now + random(1200, 3500);
  }
  if (now >= nextMicro) {
    tgtX = constrain(tgtX + random(-6, 7) / 100.0, -1.0, 1.0);
    tgtY = constrain(tgtY + random(-4, 5) / 100.0, -0.7, 0.7);
    nextMicro = now + random(400, 1100);
  }
  if (durmiendo) { tgtX = 0; tgtY = 0.35f; nextLook = now + 1000; }   // dormido: mirada quieta, un poco abajo
  if (pensandoIA) { tgtX = 0.6f; tgtY = -0.6f; nextLook = now + 800; }   // pensando
  if (!pasado(escuchaHasta)) {                         // te escucha: te mira, ojos grandes que brincan con tu voz
    tgtX = 0; tgtY = -0.25f; nextLook = now + 1000;
    float entra = suave((now - escuchaDesde) / 300.0f);
    escX *= 1.0f + entra * (0.15f + 0.15f * vozNivel);   // 3.6: mucho mas notorio
    escY *= 1.0f + entra * (0.25f + 0.35f * vozNivel);
    offY -= (int)(entra * (4.0f + 6.0f * vozNivel));
  } else if (escuchaDesde) {                           // te callaste: asiente ("entendido")
    if (now - escuchaDesde > ESCUCHA_MS + 1500 && reactUntil == 0 && manualUntil == 0) iniciarGesto(G_SALTITO, now);
    escuchaDesde = 0;
  }
  if (fabsf(tiltS) > 2.0f || fabsf(frenteS) > 2.0f) {  // inclinado: mira hacia el lado bajo
    tgtX = constrain(tiltS / 18.0f, -1.0f, 1.0f);
    tgtY = constrain(frenteS / 22.0f, -1.0f, 1.0f) * 0.7f;
    nextLook = now + 600;
  }
  lookVX = (lookVX + (tgtX - lookX) * 0.16f) * 0.68f;
  lookVY = (lookVY + (tgtY - lookY) * 0.16f) * 0.68f;
  lookX += lookVX;
  lookY += lookVY;

  // --- parpadeo (mas seguido si esta cansado; nunca a media bostezada) ---
  if (!bostezando && !blinking && !durmiendo && now >= nextBlink) { blinking = true; blinkStart = now; }
  if (blinking) {
    uint32_t t = now - blinkStart;
    if (t >= 180) {
      blinking = false;
      openAmt = 1.0;
      uint32_t espera = random(2000, 6000);
      if (cansancio < 0.9f) espera = espera * 6 / 10;
      nextBlink = (random(4) == 0 || dobleParpadeo) ? now + 220 : now + espera;
      dobleParpadeo = false;
    } else {
      openAmt = fabs(cos(PI * t / 180.0));
    }
  }
}

// 3.7/3.9: boquita de 5 barritas. Sigue a la cara (mirada, respiracion, brincos, inclinacion).
//   te escucha  -> sube y baja con tu voz          pensando IA -> tres puntitos
//   habla la IA -> se abre y se cierra             interacciones (3.9) -> sonrisa, risa, "o",
//   dientes apretados (YA!), "O" (susto), ondulada (mareado), bostezo
void drawBoca() {
  uint32_t t = millis();
  bool escucha = escuchaDesde && !pasado(escuchaHasta);
  bool piensa = iaEsperaHasta && !pasado(iaEsperaHasta);
  bool habla = iaHablaDesde && t - iaHablaDesde < IA_HABLA_MS;
  uint8_t modo = (bocaModo != BOCA_NADA && !pasado(bocaHasta)) ? bocaModo : (uint8_t)BOCA_NADA;
  if (durmiendo && modo != BOCA_BOSTEZO) modo = BOCA_NADA;
  if (!escucha && !piensa && !habla && modo == BOCA_NADA) return;
  const int xL = (SPR_W * 29) / 100, xR = (SPR_W * 71) / 100;      // centros de los ojos
  int cx = SPR_W / 2 + (int)(lookX * 14) + offX;
  int base = SPR_H / 2 + 40 + (int)(lookY * 9) + offY + (int)ojo.dy;
  float uM = (modo != BOCA_NADA) ? (t - bocaDesde) / (float)(bocaHasta - bocaDesde) : 0;
  float envM = suave(uM / 0.12f) * (1.0f - suave((uM - 0.85f) / 0.15f));   // entra y sale suave
  if (modo == BOCA_PREGUNTA) cx += (int)(3 * sinf(t / 150.0f));          // la "o" se mueve de lado a lado
  for (int i = 0; i < 5; i++) {
    int x = cx - 22 + i * 10;
    float f = (float)(x + 2 - xL) / (xR - xL);                    // donde queda entre los dos ojos
    int y = base + (int)(gDy[0] + (gDy[1] - gDy[0]) * f);           // se inclina como los ojos
    float u = (i - 2) / 2.0f;                                       // -1 .. 1 (de una orilla a la otra)
    int h = 0;
    uint16_t col = COL_TURQUESA;
    if (escucha) {
      float entra = suave((t - escuchaDesde) / 300.0f);
      float fase = sinf(t / 90.0f + i * 1.3f) * 0.5f + 0.5f;
      h = 3 + (int)(entra * (4 + 20 * vozNivel * (0.4f + 0.6f * fase)));
    } else if (habla) {
      float uh = (t - iaHablaDesde) / (float)IA_HABLA_MS;
      float abre = fabsf(sinf(t / 70.0f)) * (0.6f + 0.4f * sinf(t / 23.0f + i));
      h = 3 + (int)(16 * abre * (1.0f - suave((uh - 0.8f) / 0.2f)));
    } else if (piensa) {                                            // tres puntitos
      if (i == 0 || i == 4) continue;
      y -= (int)(5 * fmaxf(0.0f, sinf(t / 160.0f - i * 1.1f)));
      h = 5;
    } else switch (modo) {
      case BOCA_SONRISA:                                            // sonrisa: orillas arriba, se mece
        y -= (int)(envM * (7 * u * u - 2));
        h = 3 + (int)(envM * (2 + 2 * fabsf(sinf(t / 180.0f + i))));
        break;
      case BOCA_RISA:                                               // risa: sonrisa que brinca
        y -= (int)(envM * (7 * u * u - 2));
        h = 3 + (int)(envM * (3 + 6 * fabsf(sinf(t / 110.0f + i * 0.9f))));   // 4.1: mas lento
        break;
      case BOCA_PREGUNTA:                                           // "o" chiquita
        if (i == 0 || i == 4) continue;
        h = 3 + (int)(envM * (i == 2 ? 3 : 6));
        break;
      case BOCA_GRR:                                                // dientes apretados, coral
        y += (i % 2 ? 2 : -2) * (((t / 70) % 2) ? 1 : -1);
        h = 3 + (int)(envM * 3);
        col = COL_CORAL;
        break;
      case BOCA_O:                                                  // "O" de susto
        if (i == 0 || i == 4) continue;
        h = 3 + (int)(envM * (i == 2 ? 6 : 14));
        break;
      case BOCA_ONDA:                                               // mareado: ola
        y += (int)(envM * 5 * sinf(t / 80.0f + i * 1.2f));
        h = 4;
        break;
      case BOCA_BOSTEZO: {                                          // se abre grande y se cierra
        if (i == 0 || i == 4) continue;
        float a = sinf(PI * fminf(1.0f, uM));
        h = 3 + (int)(a * (i == 2 ? 20 : 15));
        break;
      }
      default: continue;
    }
    if (h > 0) spr.fillRect(x, y - h, 5, h, col);
  }
}

// 3.9: zzz flotando (aburrido o dormido)
void zeta(int x, int y, int s, uint16_t c) {
  for (int d = 0; d < 2; d++) {
    spr.drawLine(x, y + d, x + s, y + d, c);
    spr.drawLine(x + s, y + d, x, y + s + d, c);
    spr.drawLine(x, y + s + d, x + s, y + s + d, c);
  }
}
void drawZetas() {
  if (!(durmiendo && dormirAmt > 0.5f) && aburridoAmt < 0.5f) return;
  uint32_t t = millis();
  bool grandes = durmiendo;
  for (int k = 0; k < 3; k++) {
    float p = fmodf(t / 2400.0f + k / 3.0f, 1.0f);
    if (p > 0.85f) continue;                                        // se desvanece arriba
    int sz = (grandes ? 8 : 6) + (int)((grandes ? 8 : 6) * p);
    int x = (SPR_W * 71) / 100 + 30 + (int)(16 * p) + offX;
    int y = 36 - (int)(32 * p) + offY / 2;
    zeta(x, y, sz, COL_TURQUESA);
  }
}

void drawEyes() {
  spr.fillSprite(colFondo);
  drawEye((SPR_W * 29) / 100, SPR_H / 2, 0);
  drawEye((SPR_W * 71) / 100, SPR_H / 2, 1);
  drawBoca();
  drawZetas();
  spr.pushSprite(0, SPR_Y);
}

void ojosPie(uint32_t now) {
  char txt[100] = "";
  uint16_t c = COL_MARFIL;
  int p = -1;
  if (estado == ST_CALENTANDO) {
    uint32_t r1 = 0, r2 = 0, rem = 0;
    int a = progPMS(now, r1), b = progSCD(now, r2);
    int pmin = 101;
    if (a >= 0) { pmin = a; rem = r1; }
    if (b >= 0 && b < pmin) pmin = b;
    if (b >= 0 && r2 > rem) rem = r2;
    char t[12];
    fmtTiempo(t, sizeof(t), rem);
    snprintf(txt, sizeof(txt), "Calentando sensores  %s", t);
    c = COL_TURQUESA;
    p = pmin <= 100 ? pmin : -1;
  } else {
    uint16_t cc;
    const char *m = consejo(cc);
    bool falla = (cc == COL_CORAL && strncmp(m, "Sin datos", 9) == 0);
    // falla de sensor: manda la regla. En Alerta, la IA solo si su mensaje es posterior al inicio de la alerta
    bool iaVieja = (estado == ST_ALERTA) && (int32_t)(iaRecibidoMs - estadoDesde) < 0;
    if (falla || (iaVieja && !(iaEsperaHasta && !pasado(iaEsperaHasta)))) {
      strncpy(txt, m, sizeof(txt) - 1);
      txt[sizeof(txt) - 1] = '\0';
      c = cc;
    } else if (msgHasta && !pasado(msgHasta)) {     // 4.0: buenos dias, ventilaste, resumen, pausa
      strncpy(txt, msgTxt, sizeof(txt) - 1);
      txt[sizeof(txt) - 1] = '\0';
      c = msgCol;
    } else if (iaEsperaHasta && !pasado(iaEsperaHasta)) {
      snprintf(txt, sizeof(txt), "Preguntando a la IA...");
      c = COL_TURQUESA;
    } else if (iaVigente()) {
      strncpy(txt, iaTxt, sizeof(txt) - 1);
      txt[sizeof(txt) - 1] = '\0';
      c = COL_TURQUESA;
    } else if (!pasado(avisoHasta)) {
      strncpy(txt, m, sizeof(txt) - 1);
      txt[sizeof(txt) - 1] = '\0';
      c = cc;
    }
    if (txt[0] == '\0' && durmiendo && dormirAmt > 0.5f) { strcpy(txt, "Zzz..."); c = COL_TURQUESA; }
  }
  pie(txt, c, p);
}

void enterEyesMode() {
  uint32_t now = millis();
  nextBlink = now + 1500;
  nextLook = now + 800;
  reactUntil = 0;
  expr = exprAnimo;
  havePrev = false;
  shakeMs = 0; energia = 0; acN = 0; acSX = acSY = acSZ = 0;
  estadoAnim = estado;
  co2Alto = !isnan(co2Efectivo) && co2Efectivo >= 1000;
  ventanaVista = evVentanaHasta;
  lookVX = lookVY = 0;
  acDeltaMax = 0; serieToques = 0; animTipo = AN_NADA; empeoroPend = false;
  chueco = false; incomodo = false; chuecoDesde = 0;
  durmiendo = false; dormirAmt = 0;
  ultimaInteraccion = now;     // regresar a OJOS es hacerle caso
}

// =====================================================================
//                              DATOS
// =====================================================================
void tile(int x, int y, const char *lab, const char *val, uint16_t col) {
  spr.setTextDatum(TL_DATUM);
  spr.setTextColor(COL_TURQUESA, colFondo);
  spr.drawString(lab, x, y, 2);
  spr.setTextColor(col, colFondo);
  spr.drawString(val, x, y + 16, 4);
}

uint8_t datosPag = 0;   // 0 = principal, 1 = gas estimado

void drawDatos(uint32_t now) {
  char v[16], s[24];
  spr.fillSprite(colFondo);
  spr.setTextDatum(TL_DATUM);

  if (datosPag == 0) {
    // --- CO2 grande ---
    spr.setTextColor(COL_TURQUESA, colFondo);
    spr.drawString("CO2  ppm", 10, 0, 2);
    bool co2Ok = scdCom && scdN > 0;
    if (co2Ok) snprintf(v, sizeof(v), "%u", (unsigned)scdCO2); else strcpy(v, "--");
    spr.setTextColor(co2Ok ? colNivel(vCO2.nivel) : COL_ISOBARA, colFondo);
    spr.drawString(v, 10, 16, FUENTE_GRANDE);
    if (scdSt == SS_CALENTANDO) strcpy(s, "calentando");
    else if (isnan(pendCO2)) strcpy(s, co2Ok ? "midiendo tendencia" : "");
    else if (pendCO2 >= 5) snprintf(s, sizeof(s), "sube %d ppm/min", (int)(pendCO2 + 0.5f));
    else if (pendCO2 <= -5) snprintf(s, sizeof(s), "baja %d ppm/min", (int)(-pendCO2 + 0.5f));
    else strcpy(s, "estable");
    spr.setTextColor(COL_MARFIL, colFondo);
    spr.drawString(s, 10, 66, 2);

    // --- PM2.5 grande ---
    spr.setTextColor(COL_TURQUESA, colFondo);
    spr.drawString("PM2.5  ug/m3", 170, 0, 2);
    bool pmOk = pmsFrames > 0 && !sensorFalla(pmsSt);
    if (pmOk) snprintf(v, sizeof(v), "%u", (unsigned)pm25); else strcpy(v, "--");
    spr.setTextColor(pmOk ? colNivel(vPM25.nivel) : COL_ISOBARA, colFondo);
    spr.drawString(v, 170, 16, FUENTE_GRANDE);
    if (pmsSt == SS_CALENTANDO) strcpy(s, "calentando");
    else if (vPM25.ema.ok) snprintf(s, sizeof(s), "prom. %d", (int)(vPM25.ema.v + 0.5f));
    else s[0] = '\0';
    spr.setTextColor(COL_MARFIL, colFondo);
    spr.drawString(s, 170, 66, 2);

    spr.drawFastHLine(10, 86, 300, COL_ISOBARA);

    // --- fila de abajo ---
    bool tOk = ahtSt == SS_OK && !isnan(ahtT);
    fmt1(v, sizeof(v), tOk ? ahtT : NAN);
    tile(10, 92, "Temp C", v, tOk ? COL_MARFIL : COL_ISOBARA);
    if (tOk) snprintf(v, sizeof(v), "%d", (int)(ahtRH + 0.5f)); else strcpy(v, "--");
    tile(88, 92, "Humedad %", v, tOk ? COL_MARFIL : COL_ISOBARA);
    if (pmOk) snprintf(v, sizeof(v), "%u", (unsigned)pm10); else strcpy(v, "--");
    tile(166, 92, "PM10", v, pmOk ? colNivel(vPM10.nivel) : COL_ISOBARA);
    if (pmOk) snprintf(v, sizeof(v), "%u", (unsigned)pm1); else strcpy(v, "--");
    tile(244, 92, "PM1", v, pmOk ? COL_MARFIL : COL_ISOBARA);
  } else {
    // --- pagina 2: gas (ENS160), valores estimados ---
    bool gOk = ensSt == SS_OK && ensN > 0;
    if (gOk) snprintf(v, sizeof(v), "%u", (unsigned)ensAQI); else strcpy(v, "--");
    tile(10, 4, "AQI 1-5", v, gOk ? colNivel(vAQI.nivel) : COL_ISOBARA);
    if (gOk) snprintf(v, sizeof(v), "%u", (unsigned)ensTVOC); else strcpy(v, "--");
    tile(170, 4, "TVOC ppb", v, gOk ? COL_MARFIL : COL_ISOBARA);
    if (gOk) snprintf(v, sizeof(v), "%u", (unsigned)ensECO2); else strcpy(v, "--");
    tile(10, 56, "eCO2 estimado ppm", v, gOk ? COL_MARFIL : COL_ISOBARA);
    if (ensSt == SS_OK) { strcpy(v, "listo"); tile(170, 56, "Sensor de gas", v, COL_LIMA); }
    else if (ensSt == SS_CALENTANDO) {
      uint32_t dur = (ensFlag == 2) ? ENS_STARTUP_MS : ENS_WARMUP_MS;
      uint32_t el = now - ensT0;
      if (el < dur) fmtTiempo(v, sizeof(v), dur - el); else strcpy(v, "pronto");
      tile(170, 56, ensFlag == 2 ? "Gas: 1a vez" : "Gas: calentando", v, COL_TURQUESA);
    } else { strcpy(v, "falla"); tile(170, 56, "Sensor de gas", v, COL_CORAL); }
    spr.setTextColor(COL_TURQUESA, colFondo);
    spr.drawString("Valores estimados: no se usan para alertar.", 10, 112, 2);
  }
  spr.setTextDatum(TR_DATUM);
  spr.setTextColor(COL_ISOBARA, colFondo);
  spr.drawString(datosPag == 0 ? "1/2" : "2/2", 310, 148, 1);
  spr.setTextDatum(TL_DATUM);
  spr.pushSprite(0, SPR_Y);

  headerDerecha(EST_TXT[estado], colEstado(estado));
  uint16_t cc;
  const char *m = consejo(cc);
  pie(m, cc, -1);
}

// =====================================================================
//                            HISTORICO
// =====================================================================
void fmtSerie(char *b, size_t n, const Serie &s, int16_t raw) {
  if (raw == HIST_VACIO) { snprintf(b, n, "--"); return; }
  if (s.escala == 10) fmt1(b, n, raw / 10.0f);
  else snprintf(b, n, "%d", raw);
}

uint16_t mezcla(uint16_t a, uint16_t b, float f) {   // f = cuanto de a (0..1)
  float ar, ag, ab, br, bg, bb;
  colorA(a, ar, ag, ab); colorA(b, br, bg, bb);
  return colorDe(ar * f + br * (1 - f), ag * f + bg * (1 - f), ab * f + bb * (1 - f));
}
uint16_t colNivel5(int n) {   // Feliz .. Alerta, con la paleta de NIMBO
  switch (n) {
    case 4: return COL_LIMA;
    case 3: return mezcla(COL_LIMA, COL_TURQUESA, 0.5f);
    case 2: return COL_MARFIL;
    case 1: return mezcla(COL_CORAL, COL_MARFIL, 0.6f);
    default: return COL_CORAL;
  }
}

// 3.9: HISTORICO con mas vida: valor grande en su color, nivel, min/max, franjas, relleno y linea gruesa
void drawHist() {
  const Serie &s = *SERIES[histVar];
  const int gx0 = 34, gy0 = 34, gh = 104;             // grafica: 276 x 104
  const int gy1 = gy0 + gh;
  char b[40], b2[16];

  spr.fillSprite(colFondo);
  spr.setTextDatum(TL_DATUM);

  int16_t mn = 32767, mx = -32768, ult = HIST_VACIO;
  uint16_t iMn = 0, iMx = 0;
  for (uint16_t i = 0; i < histCount; i++) {
    int16_t x = histGet(s, i);
    if (x == HIST_VACIO) continue;
    if (x < mn) { mn = x; iMn = i; }
    if (x > mx) { mx = x; iMx = i; }
    ult = x;
  }
  auto colDe = [&](int16_t raw) -> uint16_t {
    return s.u ? colNivel5(nivelCrudo(s.u, raw / (float)s.escala)) : COL_TURQUESA;
  };

  if (mx < mn) {
    spr.setTextDatum(MC_DATUM);
    spr.setTextColor(COL_MARFIL, colFondo);
    spr.drawString("Juntando datos...", 160, 70, 2);
    snprintf(b, sizeof(b), "1 punto cada %s", ajHist ? "1 min" : "10 s");
    spr.setTextColor(COL_TURQUESA, colFondo);
    spr.drawString(b, 160, 92, 2);
    spr.setTextDatum(TL_DATUM);
  } else {
    // ---- encabezado: valor grande en su color, unidad, nivel; min y max a la derecha ----
    uint16_t cU = colDe(ult);
    fmtSerie(b2, sizeof(b2), s, ult);
    spr.setTextColor(cU, colFondo);
    spr.drawString(b2, 6, 3, 4);
    int xx = 6 + spr.textWidth(b2, 4) + 4;
    spr.setTextColor(COL_TURQUESA, colFondo);
    spr.drawString(s.unidad, xx, 12, 2);
    xx += spr.textWidth(s.unidad, 2) + 8;
    if (s.u) {                                          // etiqueta del nivel (fondo de color, letra oscura)
      int n = nivelCrudo(s.u, ult / (float)s.escala);
      const char *nom = EST_TXT[n];
      int wn = spr.textWidth(nom, 2) + 12;
      spr.fillRoundRect(xx, 7, wn, 19, 9, cU);
      spr.setTextColor(colFondo, cU);
      spr.drawString(nom, xx + 6, 9, 2);
    }
    spr.setTextDatum(TR_DATUM);
    fmtSerie(b2, sizeof(b2), s, mx);
    spr.setTextColor(colDe(mx), colFondo);
    spr.drawString(b2, 314, 4, 2);
    spr.setTextColor(COL_ISOBARA, colFondo);
    spr.drawString("max", 314 - spr.textWidth(b2, 2) - 4, 7, 1);
    fmtSerie(b2, sizeof(b2), s, mn);
    spr.setTextColor(colDe(mn), colFondo);
    spr.drawString(b2, 314, 18, 2);
    spr.setTextColor(COL_ISOBARA, colFondo);
    spr.drawString("min", 314 - spr.textWidth(b2, 2) - 4, 21, 1);
    spr.setTextDatum(TL_DATUM);

    // ---- escala ----
    const int16_t SPAN_MIN[4] = {200, 20, 20, 100};   // rango minimo (unidades guardadas)
    int32_t lo = mn, hi = mx;
    int32_t span = hi - lo;
    if (span < SPAN_MIN[histVar]) { int32_t c = (lo + hi) / 2; lo = c - SPAN_MIN[histVar] / 2; hi = lo + SPAN_MIN[histVar]; }
    int32_t pad = (hi - lo) / 8 + 1;
    lo -= pad; hi += pad;
    if (lo < 0 && histVar != 2) lo = 0;               // solo la T puede ser negativa
    span = hi - lo;
    auto Y = [&](int32_t v) -> int { return gy1 - (int)((v - lo) * gh / span); };

    // ---- franjas de color por nivel (fondo) + su tira y numero a la izquierda ----
    if (s.u) {
      int32_t cortes[6]; int nc = 0;
      cortes[nc++] = lo;
      for (int k = 0; k < 4; k++) { int32_t th = (int32_t)(s.u[k] * s.escala); if (th > lo && th < hi) cortes[nc++] = th; }
      cortes[nc++] = hi;
      for (int k = 0; k < nc - 1; k++) {
        int32_t mid = (cortes[k] + cortes[k + 1]) / 2;
        uint16_t c = colDe((int16_t)mid);
        int ya = Y(cortes[k + 1]), yb = Y(cortes[k]);
        spr.fillRect(gx0, ya, HIST_N, yb - ya, mezcla(c, colFondo, 0.10f));
        spr.fillRect(gx0 - 4, ya + 1, 2, yb - ya - 1, c);
      }
      for (int k = 0; k < 4; k++) {
        int32_t th = (int32_t)(s.u[k] * s.escala);
        if (th <= lo || th >= hi) continue;
        int y = Y(th);
        uint16_t c = colDe((int16_t)(th + 1));
        for (int x = gx0; x < gx0 + HIST_N; x += 6) spr.drawFastHLine(x, y, 3, mezcla(c, colFondo, 0.35f));
        snprintf(b, sizeof(b), "%ld", (long)(th / s.escala));
        spr.setTextColor(c, colFondo);
        spr.drawString(b, 2, y - 4, 1);
      }
    }

    // ---- relleno bajo la linea (color del nivel, oscurecido) ----
    for (uint16_t i = 0; i < histCount; i++) {
      int16_t v = histGet(s, i);
      if (v == HIST_VACIO) continue;
      int sx = gx0 + (HIST_N - histCount) + i, sy = Y(v);
      if (sy < gy1) spr.drawFastVLine(sx, sy, gy1 - sy, mezcla(colDe(v), colFondo, 0.30f));
    }
    // ---- linea gruesa (cada tramo con el color de su nivel) ----
    int px = -1, py = 0;
    for (uint16_t i = 0; i < histCount; i++) {
      int16_t v = histGet(s, i);
      int sx = gx0 + (HIST_N - histCount) + i;
      if (v == HIST_VACIO) { px = -1; continue; }
      int sy = Y(v);
      uint16_t c = colDe(v);
      if (px >= 0) { spr.drawLine(px, py, sx, sy, c); spr.drawLine(px, py - 1, sx, sy - 1, c); }
      px = sx; py = sy;
    }
    // ---- marcas de maximo y minimo ----
    {
      int sx = gx0 + (HIST_N - histCount) + iMx, sy = Y(mx);
      spr.fillTriangle(sx - 3, sy - 8, sx + 3, sy - 8, sx, sy - 3, colDe(mx));
      sx = gx0 + (HIST_N - histCount) + iMn; sy = Y(mn);
      spr.fillTriangle(sx - 3, sy + 8, sx + 3, sy + 8, sx, sy + 3, colDe(mn));
    }
    // ---- punto de "ahora" ----
    if (px >= 0) {
      spr.drawCircle(px, py, 6, mezcla(cU, colFondo, 0.5f));
      spr.fillCircle(px, py, 3, cU);
    }
  }

  // ---- eje de tiempo ----
  spr.drawFastHLine(gx0, gy1, HIST_N, COL_ISOBARA);
  uint32_t spanMin = (uint32_t)HIST_N * histPeriodoMs() / 60000UL;
  if (spanMin >= 120) snprintf(b, sizeof(b), "-%lu h", (unsigned long)(spanMin / 60));
  else snprintf(b, sizeof(b), "-%lu min", (unsigned long)spanMin);
  spr.setTextColor(COL_TURQUESA, colFondo);
  spr.drawString(b, gx0, gy1 + 5, 1);
  spr.setTextDatum(TR_DATUM);
  spr.drawString("ahora", gx0 + HIST_N, gy1 + 5, 1);
  spr.setTextDatum(TL_DATUM);
  spr.pushSprite(0, SPR_Y);

  snprintf(b, sizeof(b), "%s %s", s.nombre, s.unidad);
  headerDerecha(b, COL_MARFIL);
  pie("Joystick: cambiar variable", COL_TURQUESA, -1);
}

// =====================================================================
//                             AJUSTES
// =====================================================================
enum { AJ_SONIDO, AJ_SENS, AJ_OFFSET, AJ_HR, AJ_CALIB, AJ_PRUEBA, AJ_N };
const char *const AJ_TXT[AJ_N] = {"Sonido", "Sensibilidad", "Ajuste de temp.", "Ajuste de humedad",
                                  "Calibrar CO2", "Probar interaccion"};

void recalcularTH() {   // despues de cambiar un ajuste: recalcula T y HR con la ultima lectura cruda
  if (isnan(ahtTraw) || isnan(ahtRHraw)) return;
  ahtT = ahtTraw + offsetT;
  float hc = ahtRHraw * presionVapor(ahtTraw) / presionVapor(ahtT);
  ahtRHcomp = hc;
  ahtRH = constrain(hc + offsetHR, 0.0f, 100.0f);
}

void ajValor(uint8_t i, char *b, size_t n, uint32_t now) {
  static const char *const SENS4[4] = {"apagada", "baja", "media", "alta"};
  switch (i) {
    case AJ_SONIDO:   snprintf(b, n, "%s", mute ? "no" : "si"); break;
    case AJ_SENS:     snprintf(b, n, "%s", SENS4[ajSens]); break;
    case AJ_OFFSET: {   // con la temperatura que queda, para compararla con tu termometro
      char t[10], v[10]; fmt1(t, sizeof(t), offsetT); fmt1(v, sizeof(v), ahtSt == SS_OK ? ahtT : NAN);
      snprintf(b, n, "%s C  (= %s C)", t, v); break;
    }
    case AJ_HR: {       // con la humedad que queda, para compararla con tu medidor
      if (ahtSt == SS_OK && !isnan(ahtRH)) snprintf(b, n, "%+d  (= %d %%)", (int)offsetHR, (int)(ahtRH + 0.5f));
      else snprintf(b, n, "%+d  (= -- %%)", (int)offsetHR);
      break;
    }
    case AJ_PRUEBA: snprintf(b, n, calHecha ? "hecha (izq: borrar)" : "CLIC para empezar"); break;
    case AJ_CALIB:
      if (!pasado(calibMsgHasta)) snprintf(b, n, "%s", calibMsg);
      else if (ajSel == AJ_CALIB && btnState[BT_CLIC] && !calibHecho) {
        uint32_t h = now - btnCambio[BT_CLIC];
        snprintf(b, n, "suelta en %lu s", (unsigned long)(h < 3000 ? (3000 - h + 999) / 1000 : 0));
      } else snprintf(b, n, "afuera: CLIC 3 s");
      break;
    default: b[0] = '\0';
  }
}

void drawAjustes(uint32_t now) {
  char b[40];
  spr.fillSprite(colFondo);
  for (uint8_t i = 0; i < AJ_N; i++) {
    int y = 6 + i * 22;
    bool sel = (i == ajSel);
    uint16_t bg = sel ? COL_ISOBARA : colFondo;
    if (sel) spr.fillRect(4, y - 3, 312, 21, COL_ISOBARA);
    spr.setTextDatum(TL_DATUM);
    spr.setTextColor(sel ? COL_LIMA : COL_MARFIL, bg);
    spr.drawString(AJ_TXT[i], 10, y, 2);
    ajValor(i, b, sizeof(b), now);
    spr.setTextDatum(TR_DATUM);
    spr.setTextColor(sel ? COL_LIMA : COL_TURQUESA, bg);
    spr.drawString(b, 310, y, 2);
  }
  char t[10];
  fmt1(t, sizeof(t), ahtSt == SS_OK ? ahtTraw : NAN);
  snprintf(b, sizeof(b), "T cruda %s  microSD %s  RAM libre %d KB", t, sdOk ? "si" : "no", ramLibre() / 1024);
  spr.setTextDatum(TL_DATUM);
  spr.setTextColor(COL_TURQUESA, colFondo);
  spr.drawString(b, 10, 146, 1);
  spr.pushSprite(0, SPR_Y);
  pie("Arriba/abajo: elegir   Izq/der: cambiar", COL_TURQUESA, -1);
}

void ajCambiar(int dir, uint32_t now) {
  (void)now;
  switch (ajSel) {
    case AJ_SONIDO: mute = !mute; break;
    case AJ_SENS:   ajSens = (uint8_t)((ajSens + dir + 4) % 4); clickUmbral(); break;
    case AJ_OFFSET:
      offsetT = floorf((offsetT + 0.1f * dir) * 10.0f + 0.5f) / 10.0f;
      if (offsetT < -12) offsetT = -12;
      if (offsetT > 3) offsetT = 3;
      recalcularTH();
      break;
    case AJ_HR:
      offsetHR += dir;
      if (offsetHR < -30) offsetHR = -30;
      if (offsetHR > 10) offsetHR = 10;
      recalcularTH();
      break;
    case AJ_PRUEBA:   // borrar lo que midio la prueba guiada: vuelve a los valores de fabrica
      if (calHecha) {
        calHecha = false; vozFacCal = sorpFacCal = sorpPisoCal = 0; vozSorda = false;
        tapThr = TAP_THR; signoX = TILT_SIGNO_X; signoY = TILT_SIGNO_Y;
      }
      break;
    default: break;
  }
  ajustesSucios = millis() | 1;   // 3.8: se guarda en la microSD 3 s despues
}

void calibrarCO2(uint32_t now) {
  calibMsgHasta = now + 5000;
  if (!scdCom || scdSt != SS_OK || now - scdT0 < 120000UL) {
    snprintf(calibMsg, sizeof(calibMsg), "espera 2 min de SCD30");
    return;
  }
  scd30.setForcedRecalibrationFactor(CO2_FRC_PPM);
  Wire.setClock(I2C_HZ);
  vCO2.ema.ok = false; co2UltN = 0;
  snprintf(calibMsg, sizeof(calibMsg), "hecho: %d ppm", CO2_FRC_PPM);
  Serial.print(F("# calibracion forzada CO2 a "));
  Serial.print(CO2_FRC_PPM);
  Serial.print(F(" ppm (lectura previa "));
  Serial.print(scdCO2);
  Serial.println(F(")"));
  if (!mute) beepSeq(2, 1800, 80, 80, 0);
}

// =====================================================================
//                              TEST
// =====================================================================
void enterTestMode() {
  tft.fillScreen(colFondo);
  tft.setTextDatum(TL_DATUM);
  tft.setTextColor(COL_LIMA, colFondo);
  tft.setTextPadding(0);
  tft.drawString("TEST WIO TERMINAL", 10, 6, 4);
  tft.setTextColor(COL_TURQUESA, colFondo);
  tft.drawString("A: ojos   B: pitido   CLIC: prueba guiada", 10, 215, 2);
  tft.setTextColor(COL_MARFIL, colFondo);
  tft.drawString("Acelerometro", 10, 40, 2);
  tft.drawString("Inclinacion", 10, 70, 2);
  tft.drawString("Microfono", 10, 100, 2);
  tft.drawString("Botones", 10, 135, 2);
  tft.drawString("Joystick", 10, 160, 2);
  tft.drawString("Golpe pico", 10, 185, 2);
}

uint32_t lastTest = 0, lastLed = 0;
bool ledState = false;

void updateTest() {
  uint32_t now = millis();
  if (now - lastLed >= 500) { lastLed = now; ledState = !ledState; digitalWrite(LED_BUILTIN, ledState); }

  bool b = (digitalRead(PIN_BTN_B) == LOW);
  if (digitalRead(WIO_5S_UP) == LOW)         beep(1000, 120);
  else if (digitalRead(WIO_5S_DOWN) == LOW)  beep(500, 120);
  else if (digitalRead(WIO_5S_LEFT) == LOW)  beep(700, 120);
  else if (digitalRead(WIO_5S_RIGHT) == LOW) beep(1400, 120);
  else if (digitalRead(WIO_5S_PRESS) == LOW) beep(2000, 120);
  else if (b)                                beep(880, 120);

  acelRapido(now);
  if (now - lastTest < 100) return;
  lastTest = now;

  tft.setTextDatum(TL_DATUM);
  tft.setTextPadding(180);
  tft.setTextColor(COL_LIMA, colFondo);
  char buf[48];
  if (accelOk) {
    float x = lis.getAccelerationX(), y = lis.getAccelerationY(), z = lis.getAccelerationZ();
    snprintf(buf, sizeof(buf), "X:%5.2f Y:%5.2f Z:%5.2f g", x, y, z);
  } else snprintf(buf, sizeof(buf), "NO detectado");
  tft.drawString(buf, 130, 40, 2);

  if (accelOk) {
    float a1, a2;
    angulos(acX, acY, acZ, a1, a2);
    snprintf(buf, sizeof(buf), "izq/der %+d  adel/atr %+d", (int)a1, (int)a2);
  } else snprintf(buf, sizeof(buf), "-");
  tft.drawString(buf, 130, 70, 2);

  medirMic(buzzing);
  int ts = umbralSorpresa();
  snprintf(buf, sizeof(buf), "%d fondo %d %s", micAhora, (int)ruidoBase,
           (ts > 0 && micAhora > ts) ? "SUSTO" : vozAhora ? "VOZ" : "");
  tft.drawString(buf, 130, 100, 2);
  drawBar(130, 118, 180, 6, micAhora, ts > 0 ? ts : 1500, COL_TURQUESA);
  tft.setTextPadding(0);
  tft.setTextColor(COL_TURQUESA, colFondo);
  snprintf(buf, sizeof(buf), "voz>%d susto>%d   ", (int)umbralVoz(), ts);
  tft.drawString(buf, 130, 126, 1);
  tft.setTextPadding(180);
  tft.setTextColor(COL_LIMA, colFondo);

  snprintf(buf, sizeof(buf), "A:%s  B:%s  C:%s",
           digitalRead(PIN_BTN_A) == LOW ? "ON " : "off",
           digitalRead(PIN_BTN_B) == LOW ? "ON " : "off",
           digitalRead(PIN_BTN_C) == LOW ? "ON " : "off");
  tft.drawString(buf, 130, 135, 2);

  snprintf(buf, sizeof(buf), "%s%s%s%s%s",
           digitalRead(WIO_5S_UP) == LOW ? "ARRIBA " : "",
           digitalRead(WIO_5S_DOWN) == LOW ? "ABAJO " : "",
           digitalRead(WIO_5S_LEFT) == LOW ? "IZQ " : "",
           digitalRead(WIO_5S_RIGHT) == LOW ? "DER " : "",
           digitalRead(WIO_5S_PRESS) == LOW ? "CLIC" : "");
  if (buf[0] == '\0') snprintf(buf, sizeof(buf), "-");
  tft.drawString(buf, 130, 160, 2);
  if (clickLeer()) toquesHw++;
  snprintf(buf, sizeof(buf), "%.2f g  hw:%s %u", golpePico, clickHw ? "SI" : "NO", toquesHw);
  tft.drawString(buf, 130, 185, 2);
  tft.setTextPadding(0);
}

// =====================================================================
//          PRUEBA GUIADA (3.1): mide el microfono, los toques y la
//          inclinacion de ESTE NIMBO en SU carcasa y se ajusta sola
// =====================================================================
enum { PG_SILENCIO, PG_VOZ, PG_PALMADA, PG_TOQUES, PG_DERECHA, PG_ATRAS, PG_FIN };
const char *const PG_TXT1[PG_FIN] = {"Silencio, por favor", "Hablale normal", "Da UNA palmada",
                                     "Dale 3 toquecitos", "Inclinalo a TU derecha", "Inclinalo hacia atras"};
const char *const PG_TXT2[PG_FIN] = {"y no lo toques", "a unos 50 cm, sin parar", "fuerte, cerca de NIMBO",
                                     "suaves, en la carcasa", "unos 20 grados y sostenlo", "pantalla mirando al techo"};
const uint16_t PG_MS[PG_FIN] = {5000, 6000, 5000, 6000, 5000, 5000};
#define PG_PREP_MS 2500UL   // para leer la instruccion antes de que empiece a medir

uint8_t pgPaso = 0;
uint32_t pgInicio = 0, pgTick = 0, pgVivo = 0;
bool pgDibujado = false;
float pgSilSuma = 0, pgSilMax = 0, pgReposoG = 0, pgVozP75 = 0, pgVozMax = 0, pgPalma = 0;
uint32_t pgSilN = 0;
uint16_t pgVoz[160]; uint8_t pgVozN = 0;
uint16_t pgSil[130]; uint8_t pgSilK = 0; float pgSilP90 = 0;
float pgPicos[12]; uint8_t pgPicosN = 0; float pgPicoAct = 0; uint32_t pgPicoUlt = 0;
float pgSx = 0, pgSy = 0, pgSz = 0, pgAngX = 0, pgAngY = 0; uint16_t pgAngN = 0;
bool pgOkVoz = false, pgOkPalma = false, pgOkToques = false, pgOkX = false, pgOkY = false;
int pgUVoz = 0, pgUSusto = 0, pgUltMic = 0;

void ordenarF(float *v, int n) { for (int i = 1; i < n; i++) { float x = v[i]; int j = i - 1; while (j >= 0 && v[j] > x) { v[j + 1] = v[j]; j--; } v[j + 1] = x; } }
void ordenarU(uint16_t *v, int n) { for (int i = 1; i < n; i++) { uint16_t x = v[i]; int j = i - 1; while (j >= 0 && v[j] > x) { v[j + 1] = v[j]; j--; } v[j + 1] = x; } }

void textoCentrado(const char *t, int y, uint16_t col) {
  int f = tft.textWidth(t, 4) <= 310 ? 4 : 2;
  tft.setTextDatum(TC_DATUM);
  tft.setTextColor(col, colFondo);
  tft.drawString(t, 160, y, f);
  tft.setTextDatum(TL_DATUM);
}

void pruebaIniciar(uint32_t now) {
  pgPaso = 0; pgInicio = now; pgTick = now; pgDibujado = false;
  pgSilSuma = pgSilMax = pgReposoG = pgVozP75 = pgVozMax = pgPalma = 0; pgSilN = 0;
  pgVozN = 0; pgSilK = 0; pgSilP90 = 0; pgPicosN = 0; pgPicoAct = 0; pgPicoUlt = 0;
  pgSx = pgSy = pgSz = 0; pgAngN = 0; pgAngX = pgAngY = 0;
  acDeltaMax = 0;
  evento("prueba guiada: inicio");
}

void drawPruebaPaso() {
  char b[40];
  tft.fillScreen(colFondo);
  tft.setTextDatum(TL_DATUM);
  tft.setTextPadding(0);
  tft.setTextColor(COL_TURQUESA, colFondo);
  snprintf(b, sizeof(b), "PRUEBA GUIADA   paso %d de %d", pgPaso + 1, PG_FIN);
  tft.drawString(b, 10, 12, 2);
  textoCentrado(PG_TXT1[pgPaso], 78, COL_MARFIL);
  textoCentrado(PG_TXT2[pgPaso], 116, COL_TURQUESA);
  tft.setTextColor(COL_ISOBARA, colFondo);
  tft.drawString("C: salir", 10, 220, 2);
}

void pgCerrarPaso() {
  if (pgPaso == PG_TOQUES && pgPicoAct > 0 && pgPicosN < 12) { pgPicos[pgPicosN++] = pgPicoAct; pgPicoAct = 0; }
  if ((pgPaso == PG_DERECHA || pgPaso == PG_ATRAS) && pgAngN > 0) {
    float a, f;
    angulosCrudos(pgSx / pgAngN, pgSy / pgAngN, pgSz / pgAngN, a, f);
    if (pgPaso == PG_DERECHA) pgAngX = a; else pgAngY = f;
    pgSx = pgSy = pgSz = 0; pgAngN = 0;
  }
}

void pruebaCalcular() {
  float sil = pgSilN ? pgSilSuma / pgSilN : 1;
  if (sil < 1) sil = 1;
  // voz: lo normal de tu voz (percentil 75) contra el silencio tipico (percentil 90; 3.2: antes el pico)
  ordenarU(pgVoz, pgVozN);
  ordenarU(pgSil, pgSilK);
  pgSilP90 = pgSilK ? pgSil[pgSilK * 9 / 10] : sil;
  pgVozP75 = pgVozN ? pgVoz[pgVozN * 3 / 4] : 0;
  pgOkVoz = pgVozN > 10 && pgVozP75 > pgSilP90 * 1.15f + 15;
  if (pgOkVoz) {
    float thr = pgSilP90 + 0.35f * (pgVozP75 - pgSilP90);
    vozFacCal = thr / sil; vozSorda = false; pgUVoz = (int)thr;
  } else { vozSorda = true; pgUVoz = 0; }
  // susto: bastante mas que tu voz, menos que la palmada
  pgOkPalma = pgPalma > pgSilP90 * 1.8f + 40;
  if (pgOkPalma) {
    float thr = 0.6f * pgPalma;
    float minimo = pgOkVoz ? pgVozP75 * 1.3f : pgSilP90 * 2.5f;
    if (thr < minimo) thr = minimo;
    sorpPisoCal = thr; sorpFacCal = thr / sil; pgUSusto = (int)thr;
  } else pgUSusto = umbralSorpresa();
  // toques: la mitad del toque tipico, pero siempre arriba del ruido en reposo
  pgOkToques = pgPicosN >= 2;
  if (pgOkToques) {
    ordenarF(pgPicos, pgPicosN);
    float thr = 0.5f * pgPicos[pgPicosN / 2];
    float minimo = pgReposoG * 2.0f;
    if (minimo < 0.10f) minimo = 0.10f;
    tapThr = thr < minimo ? minimo : thr;
  }
  // signos: a tu derecha = los ojos a la derecha; hacia atras = miran hacia arriba
  pgOkX = fabsf(pgAngX) > 8; if (pgOkX) signoX = pgAngX > 0 ? 1 : -1;
  pgOkY = fabsf(pgAngY) > 8; if (pgOkY) signoY = pgAngY > 0 ? -1 : 1;
  ruidoBase = sil;
  calHecha = true;
  ajustesGuardar();      // 3.8: queda guardada en la microSD
  Serial.print(F("# PRUEBA mic_silencio=")); Serial.print((int)sil);
  Serial.print(F(" mic_silencio_max=")); Serial.print((int)pgSilMax);
  Serial.print(F(" mic_silencio_p90=")); Serial.print((int)pgSilP90);
  Serial.print(F(" voz_p75=")); Serial.print((int)pgVozP75);
  Serial.print(F(" voz_max=")); Serial.print((int)pgVozMax);
  Serial.print(F(" palmada=")); Serial.print((int)pgPalma);
  Serial.print(F(" reposo_g=")); Serial.print(pgReposoG, 2);
  Serial.print(F(" toques=")); Serial.print(pgPicosN);
  Serial.print(F(" tap_thr=")); Serial.print(tapThr, 2);
  Serial.print(F(" ang_der=")); Serial.print(pgAngX, 0);
  Serial.print(F(" ang_atras=")); Serial.print(pgAngY, 0);
  Serial.print(F(" signoX=")); Serial.print(signoX);
  Serial.print(F(" signoY=")); Serial.println(signoY);
  if (!mute) beepSeq(3, 900, 70, 40, 250);
}

void drawPruebaFin() {
  char b[56];
  tft.fillScreen(colFondo);
  tft.setTextDatum(TL_DATUM);
  tft.setTextPadding(0);
  tft.setTextColor(COL_LIMA, colFondo);
  tft.drawString("PRUEBA LISTA", 10, 6, 4);
  int y = 42;
  tft.setTextColor(COL_MARFIL, colFondo);
  snprintf(b, sizeof(b), "Mic: silencio %d/%d  voz %d  palmada %d", (int)(pgSilN ? pgSilSuma / pgSilN : 0), (int)pgSilP90, (int)pgVozP75, (int)pgPalma);
  tft.drawString(b, 10, y, 2); y += 22;
  if (pgOkVoz) { snprintf(b, sizeof(b), "Voz: OK (umbral %d)", pgUVoz); tft.setTextColor(COL_LIMA, colFondo); }
  else { snprintf(b, sizeof(b), "Voz: no te oye. Destapa el microfono"); tft.setTextColor(COL_CORAL, colFondo); }
  tft.drawString(b, 10, y, 2); y += 22;
  if (pgOkPalma) { snprintf(b, sizeof(b), "Susto: OK (umbral %d)", pgUSusto); tft.setTextColor(COL_LIMA, colFondo); }
  else { snprintf(b, sizeof(b), "Palmada: no se oyo fuerte"); tft.setTextColor(COL_CORAL, colFondo); }
  tft.drawString(b, 10, y, 2); y += 22;
  if (pgOkToques) { snprintf(b, sizeof(b), "Toques: %d sentidos (umbral %.2f g)", pgPicosN, tapThr); tft.setTextColor(COL_LIMA, colFondo); }
  else { snprintf(b, sizeof(b), "Toques: no los senti (reposo %.2f g)", pgReposoG); tft.setTextColor(COL_CORAL, colFondo); }
  tft.drawString(b, 10, y, 2); y += 22;
  snprintf(b, sizeof(b), "Inclinar: derecha %s (%+d)  atras %s (%+d)", pgOkX ? "OK" : "no", (int)pgAngX, pgOkY ? "OK" : "no", (int)pgAngY);
  tft.setTextColor((pgOkX && pgOkY) ? COL_LIMA : COL_CORAL, colFondo);
  tft.drawString(b, 10, y, 2); y += 26;
  tft.setTextColor(COL_TURQUESA, colFondo);
  tft.drawString("Ya quedo ajustado. Mandale foto a Claude", 10, y, 2);
  tft.drawString("para dejarlo fijo (se borra al apagar).", 10, y + 18, 2);
  tft.setTextColor(COL_ISOBARA, colFondo);
  tft.drawString("A: ojos   CLIC: repetir", 10, 222, 2);
}

void pruebaPaso(uint32_t now) {
  acelRapido(now);
  if (pgPaso >= PG_FIN) {
    if (!pgDibujado) { drawPruebaFin(); pgDibujado = true; }
    return;
  }
  if (!pgDibujado) { drawPruebaPaso(); pgDibujado = true; }
  uint32_t t = now - pgInicio, total = PG_PREP_MS + PG_MS[pgPaso];
  if (now - pgTick >= 40) {
    pgTick = now;
    bool midiendo = t >= PG_PREP_MS;
    bool buzz = buzzing || (now - buzzEndMs < 250);
    if (midiendo && !buzz) {
      switch (pgPaso) {
        case PG_SILENCIO: {
          int m = micNivel(); pgUltMic = m;
          pgSilSuma += m; pgSilN++;
          if (pgSilK < 130) pgSil[pgSilK++] = (uint16_t)m;
          if (m > pgSilMax) pgSilMax = m;
          if (acDeltaMax > pgReposoG) pgReposoG = acDeltaMax;
          break;
        }
        case PG_VOZ: {
          int m = micNivel(); pgUltMic = m;
          if (pgVozN < 160) pgVoz[pgVozN++] = (uint16_t)m;
          if (m > pgVozMax) pgVozMax = m;
          break;
        }
        case PG_PALMADA: { int m = micNivel(); pgUltMic = m; if (m > pgPalma) pgPalma = m; break; }
        case PG_TOQUES: {
          float g = acDeltaMax, u = pgReposoG * 1.5f;
          if (u < 0.12f) u = 0.12f;
          if (g > u) {
            if (pgPicoAct > 0 && now - pgPicoUlt > 150 && pgPicosN < 12) { pgPicos[pgPicosN++] = pgPicoAct; pgPicoAct = 0; }
            if (g > pgPicoAct) pgPicoAct = g;
            pgPicoUlt = now;
          } else if (pgPicoAct > 0 && now - pgPicoUlt > 150) {
            if (pgPicosN < 12) pgPicos[pgPicosN++] = pgPicoAct;
            pgPicoAct = 0;
          }
          break;
        }
        default:   // inclinar: promedio de los ultimos 2 s
          if (t + 2000 >= total) { pgSx += acX; pgSy += acY; pgSz += acZ; pgAngN++; }
          break;
      }
    }
    acDeltaMax = 0;
  }
  if (now - pgVivo >= 200) {   // barra de tiempo + lo que esta oyendo/sintiendo
    pgVivo = now;
    drawBar(20, 160, 280, 6, (int)t, (int)total, t >= PG_PREP_MS ? COL_LIMA : COL_TURQUESA);
    char b[40];
    if (t < PG_PREP_MS) snprintf(b, sizeof(b), "preparate...");
    else if (pgPaso <= PG_PALMADA) snprintf(b, sizeof(b), "oigo: %d", pgUltMic);
    else if (pgPaso == PG_TOQUES) snprintf(b, sizeof(b), "toques sentidos: %d", pgPicosN + (pgPicoAct > 0 ? 1 : 0));
    else { float a, f; angulosCrudos(acX, acY, acZ, a, f); snprintf(b, sizeof(b), "angulo: %+d", (int)(pgPaso == PG_DERECHA ? a : f)); }
    tft.setTextDatum(TC_DATUM);
    tft.setTextPadding(200);
    tft.setTextColor(COL_MARFIL, colFondo);
    tft.drawString(b, 160, 176, 2);
    tft.setTextPadding(0);
    tft.setTextDatum(TL_DATUM);
  }
  if (t >= total) {
    pgCerrarPaso();
    pgPaso++;
    pgInicio = now;
    pgDibujado = false;
    if (pgPaso == PG_FIN) pruebaCalcular();
    else if (!mute) beep(1200, 60);
  }
}

// =====================================================================
//                     NAVEGACION ENTRE PANTALLAS
// =====================================================================
void cambiarPantalla(Screen s) {
  scr = s;
  noTone(WIO_BUZZER);
  buzzing = false;
  pieForzar = true;
  pieTxt[0] = '\0';
  proxPant = millis();
  if (s == SC_TEST) { enterTestMode(); return; }
  if (s == SC_PRUEBA) { pruebaIniciar(millis()); return; }
  tft.fillScreen(colFondo);
  if (s == SC_OJOS) { enterEyesMode(); return; }   // en OJOS no hay cabecera
  drawHeader();
}


// =====================================================================
//                         SETUP / LOOP
// =====================================================================
void sensoresIniciar(uint32_t now) {
  if (ahtIniciar()) { ahtCom = true; ahtSt = SS_OK; ahtUlt = now; ahtNext = now; evento("AHT21 detectado"); }
  else evento("AHT21 no responde en 0x38");

  if (ensIniciar()) {
    ensCom = true; ensSt = SS_CALENTANDO; ensT0 = now; ensNextPoll = now + 1000;
    Serial.print(F("# ENS160 detectado en 0x"));
    Serial.println(ensAddr, HEX);
  } else evento("ENS160 no responde en 0x53 ni 0x52");

  bool scdOk = false;
  for (uint8_t i = 0; i < 4 && !scdOk; i++) {
    if (i) delay(400);
    scdOk = scdIniciar();
  }
  if (scdOk) {
    uint32_t t = millis();
    scdCom = true; scdSt = SS_CALENTANDO; scdT0 = t; scdUlt = t; scdNextPoll = t + 1000;
    evento("SCD30 detectado");
  } else evento("SCD30 no responde en 0x61 (se reintenta cada 15 s)");

  ahtNextRetry = ensNextRetry = scdNextRetry = now + REINTENTO_MS;
}

void setup() {
  Serial.begin(115200);

  pinMode(LED_BUILTIN, OUTPUT);
  pinMode(WIO_BUZZER, OUTPUT);
  pinMode(WIO_MIC, INPUT);
  pinMode(WIO_LIGHT, INPUT);
  pinMode(WIO_KEY_A, INPUT_PULLUP);
  pinMode(WIO_KEY_B, INPUT_PULLUP);
  pinMode(WIO_KEY_C, INPUT_PULLUP);
  pinMode(WIO_5S_UP, INPUT_PULLUP);
  pinMode(WIO_5S_DOWN, INPUT_PULLUP);
  pinMode(WIO_5S_LEFT, INPUT_PULLUP);
  pinMode(WIO_5S_RIGHT, INPUT_PULLUP);
  pinMode(WIO_5S_PRESS, INPUT_PULLUP);
  analogReadResolution(12);

  tft.begin();
  tft.setRotation(3);
  tft.invertDisplay(INVERTIR_PANTALLA);
  tft.fillScreen(colFondo);

  if (spr.createSprite(SPR_W, SPR_H) == nullptr) {
    Serial.println(F("# sprite sin RAM"));
    tft.fillScreen(COL_CORAL);
    while (1) delay(1000);
  }

  lis.begin(Wire1);
  delay(100);
  accelOk = (bool)lis;
  {
    float m = sqrtf(TILT_REF_X * TILT_REF_X + TILT_REF_Y * TILT_REF_Y + TILT_REF_Z * TILT_REF_Z);
    tiltRefAng = atan2f(TILT_REF_Y, TILT_REF_X) * 57.2958f;
    tiltRefFrente = asinf(TILT_REF_Z / m) * 57.2958f;
  }
  if (accelOk) {
    lis.setOutputDataRate(LIS3DHTR_DATARATE_400HZ);   // 3.0: rapido para no perder toques
    lis.setFullScaleRange(LIS3DHTR_RANGE_2G);
    delay(20);
    clickConfigurar();     // 3.4: toques por hardware
  }
  randomSeed(analogRead(WIO_LIGHT) + micros());

  showSplash();
  if (!mute) beep(800, 80);
  uint32_t t0 = millis();

  histReset();
  for (int i = 0; i < TR_N; i++) { trCO2[i] = NAN; trPM[i] = NAN; }
  sdIniciar();          // 3.8: microSD
  ajustesCargar();
  historialCargar();
  clickUmbral();        // por si la sensibilidad guardada es otra

  Wire.begin();
  Wire.setClock(I2C_HZ);
  Serial1.begin(9600);
  pms.begin_UART(&Serial1);
  Serial.println(F("# NIMBO fase 4.2"));
  i2cScan();
  sensoresIniciar(millis());
  Wire.setClock(I2C_HZ);
  Serial.print(F("# RAM libre aprox: "));
  Serial.println(ramLibre());

  while (millis() - t0 < 1800) { buzzerUpdate(); delay(5); }

  for (int i = 0; i < NBTN; i++) { btnState[i] = false; btnEdge[i] = false; btnSuelta[i] = false; btnCambio[i] = 0; }
  proxHist = millis() + histPeriodoMs();
  cambiarPantalla(SC_OJOS);
}

void loop() {
  uint32_t now = millis();
  leerBotones(now);
  buzzerUpdate();

  // ---- sensores y analisis: siempre, en cualquier pantalla ----
  pmsPaso(now);
  i2cTurno(now);
  iaRecibir(now);
  if (pasado(proxEstado)) { proxEstado = now + 1000; estadoActualizar(now); }
  if (pasado(proxTr))     { proxTr = now + 10000; trPush(); analizar(now); }
  if (pasado(proxHist))   {
    proxHist = now + histPeriodoMs(); histTick(); historialEscribir();
    if (diaLocal() != diaCuenta) {                 // 4.0: cambio de dia: empieza el resumen de nuevo
      diaCuenta = diaLocal();
      for (int i = 0; i < 5; i++) minEstado[i] = 0;
      ventilacionesHoy = 0;
    }
    if (estado <= ST_FELIZ && minEstado[estado] < 65000) minEstado[estado]++;   // (1 punto por minuto)
  }
  if (ajustesSucios && now - ajustesSucios > 3000) { ajustesSucios = 0; ajustesGuardar(); }
  if (pasado(proxLog))    { proxLog = now + LOG_CADA_MS; logCSV(now); }

  // ---- botones globales ----
  // A: corto (al soltar) = OJOS; largo (1 s) = TEST
  if (btnState[BT_A] && !aLargoHecho && now - btnCambio[BT_A] >= A_LARGO_MS) {
    aLargoHecho = true;
    cambiarPantalla(SC_TEST);
    click();
    return;
  }
  if (btnSuelta[BT_A]) {
    bool fueLargo = aLargoHecho;
    aLargoHecho = false;
    if (!fueLargo && scr != SC_OJOS) { cambiarPantalla(SC_OJOS); return; }
    if (!fueLargo && scr == SC_OJOS) caricia(now);
  }
  if (btnEdge[BT_C]) {
    Screen sig = (scr >= SC_CICLO - 1) ? SC_OJOS : (Screen)(scr + 1);   // desde TEST tambien vuelve a OJOS
    cambiarPantalla(sig);
    click();
    return;
  }
  if (btnEdge[BT_B] && scr != SC_TEST) {
    mute = !mute;
    if (mute) { noTone(WIO_BUZZER); buzzing = false; seqRest = 0; }
    else beep(1000, 80);
    proxPant = now;
    ajustesSucios = now | 1;   // 3.8
  }
  if (scr != SC_OJOS && scr != SC_TEST && now - lastInputMs > VOLVER_OJOS_MS) { cambiarPantalla(SC_OJOS); return; }

  switch (scr) {
    case SC_OJOS:
      if (btnEdge[BT_DER] || btnEdge[BT_IZQ] || btnEdge[BT_CLIC] || btnEdge[BT_ARR] || btnEdge[BT_ABA] || btnEdge[BT_B])
        interaccion(now, false);
      acelRapido(now);   // leer seguido para no perder toques
      if (btnEdge[BT_DER] || btnEdge[BT_IZQ]) animTipo = AN_NADA;
      if (btnEdge[BT_DER]) { reactUntil = 0; setExpr((Expr)((expr % E_COUNT + 1) % E_COUNT), true); manualUntil = now + MANUAL_MS; }
      if (btnEdge[BT_IZQ]) { reactUntil = 0; setExpr((Expr)((expr % E_COUNT + E_COUNT - 1) % E_COUNT), true); manualUntil = now + MANUAL_MS; }
      if (btnEdge[BT_CLIC]) { avisoHasta = now + AVISO_MS; iaPedir(now); }
      if (btnEdge[BT_ARR] && reactUntil == 0) mostrarResumen(now);   // 4.0: resumen de hoy
      if (btnEdge[BT_ABA] && reactUntil == 0) iniciarBostezo(now);
      if (btnEdge[BT_IZQ] || btnEdge[BT_DER] || btnEdge[BT_CLIC] || btnEdge[BT_B]) {   // te mira al tocarlo
        tgtX = 0; tgtY = 0; nextLook = now + 1500;
      }
      if (now - lastFrame >= 33) { lastFrame = now; updateEyes(); drawEyes(); }
      if (pasado(proxPie)) { proxPie = now + 250; ojosPie(now); }
      break;

    case SC_DATOS:
      if (btnEdge[BT_ARR] || btnEdge[BT_ABA]) { datosPag ^= 1; proxPant = now; click(); }
      if (pasado(proxPant)) { proxPant = now + 1000; drawDatos(now); }
      break;

    case SC_HIST:
      if (btnEdge[BT_DER] || btnEdge[BT_ABA]) { histVar = (histVar + 1) % 4; proxPant = now; click(); }
      if (btnEdge[BT_IZQ] || btnEdge[BT_ARR]) { histVar = (histVar + 3) % 4; proxPant = now; click(); }
      if (pasado(proxPant)) { proxPant = now + 2000; drawHist(); }
      break;

    case SC_AJUSTES:
      if (btnEdge[BT_ABA]) { ajSel = (ajSel + 1) % AJ_N; proxPant = now; click(); }
      if (btnEdge[BT_ARR]) { ajSel = (ajSel + AJ_N - 1) % AJ_N; proxPant = now; click(); }
      if (btnEdge[BT_DER]) { ajCambiar(+1, now); proxPant = now; click(); }
      if (btnEdge[BT_IZQ]) { ajCambiar(-1, now); proxPant = now; click(); }
      if (ajSel == AJ_CALIB && btnState[BT_CLIC]) {
        if (!calibHecho && now - btnCambio[BT_CLIC] >= 3000) { calibHecho = true; calibrarCO2(now); proxPant = now; }
        if (pasado(proxPant)) proxPant = now;   // refresca la cuenta regresiva
      } else calibHecho = false;
      if (ajSel == AJ_PRUEBA && btnEdge[BT_CLIC]) { cambiarPantalla(SC_PRUEBA); return; }
      if (pasado(proxPant)) { proxPant = now + 500; drawAjustes(now); }
      break;

    case SC_PRUEBA:
      if (btnEdge[BT_CLIC] && pgPaso >= PG_FIN) { pruebaIniciar(now); break; }
      pruebaPaso(now);
      break;

    default:   // TEST
      if (btnEdge[BT_CLIC]) { cambiarPantalla(SC_PRUEBA); return; }
      updateTest();
      break;
  }
}