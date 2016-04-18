# aplicacion-Inicio
#primera parte del aplicativo 

com.example.mechalikh.myapplication paquete;

android.app.Activity importación;
android.content.Context importación;
android.content.pm.PackageManager importación;

android.content.res.Configuration importación;
android.graphics.Rect importación;
android.hardware.Camera importación;
android.hardware.Sensor importación;
android.hardware.SensorEvent importación;
android.hardware.SensorEventListener importación;
android.hardware.SensorManager importación;
android.net.Uri importación;
android.os.Bundle importación;
android.os.Environment importación;
android.util.Log importación;
android.view.Menu importación;
android.view.MenuItem importación;
android.view.Surface importación;
android.view.SurfaceHolder importación;
android.view.SurfaceView importación;
android.view.View importación;
android.widget.Button importación;
android.widget.FrameLayout importación;
android.widget.TextView importación;

java.io.File importación;
java.io.FileNotFoundException importación;
java.io.FileOutputStream importación;
java.io.IOException importación;
java.text.SimpleDateFormat importación;
java.util.ArrayList importación;
java.util.Date importación;
java.util.List importación;


MainActivity clase pública se extiende Actividad {
    Cámara mCamera privada;
    mPreview CameraPreview privada;

    @Anular
    protected void onCreate (Bundle savedInstanceState) {
        super.onCreate (savedInstanceState);
        setContentView (R.layout.activity_main);
        // Crear una instancia de la cámara
        mCamera = getCameraInstance ();
        // Crear nuestro punto de vista de vista previa y establecerlo como el contenido de nuestra actividad.

        mPreview = new CameraPreview (esto, mCamera);

        FrameLayout previsualización = (FrameLayout) findViewById (R.id.camera_preview);
        preview.addView (mPreview);
        ((SensorManager) getSystemService (Context.SENSOR_SERVICE)) registerListener. (
                nueva SensorEventListener () {
                    @Anular
                    public void onSensorChanged (evento SensorEvent) {
                        // Establecer la velocidad de bola en base a la inclinación del teléfono (ignorar eje Z)

                        Double X = (event.values ​​[1] * 9);

                        doble distancia = (1,5 * Math.tan (Math.toRadians (x)));
                        si (distancia> 0)
                        ((TextView) findViewById (R.id.textView)) setText ( "Distancia =" + String.valueOf (distancia) + "m.");
                    }
                    @Anular
                    public void onAccuracyChanged (sensor del sensor, int exactitud) {} ​​// ignorar este evento
                },
                ((SensorManager) getSystemService (Context.SENSOR_SERVICE))
                        .getSensorList (Sensor.TYPE_ACCELEROMETER) .get (0), SensorManager.SENSOR_DELAY_NORMAL);

    }
    @Anular
    protegida onPause void () {
        super.onPause (); // Si está utilizando MediaRecorder, liberar primero
        releaseCamera (); // Liberar la cámara inmediatamente en caso de pausa
    }


    releaseCamera private void () {
        si (mCamera! = null) {
            mCamera.release (); // Liberar la cámara para otras aplicaciones
            mCamera = null;
        }
    }
    @Anular
    public boolean onCreateOptionsMenu (menú Menú) {
        // Inflar el menú; esto agrega elementos a la barra de acción si está presente.
        . GetMenuInflater () inflar (R.menu.menu_main, menú);
        return true;
    }

    @Anular
    public boolean onOptionsItemSelected (elemento MenuItem) {
        // Elemento de la barra de acción de la manija hace clic aquí. La barra de acción se
        // Manipulador automáticamente los clics en el botón Inicio / Arriba, siempre
        // Como se especifica una actividad principal en AndroidManifest.xml.
        int id = item.getItemId ();

        // Noinspection SimplifiableIfStatement
        si (id == R.id.action_settings) {
            return true;
        }

        volver super.onOptionsItemSelected (punto);
    }

    checkCameraHardware private boolean (contexto Contexto) {
        si (context.getPackageManager (). hasSystemFeature (PackageManager.FEATURE_CAMERA)) {
            // Este dispositivo tiene una cámara
            return true;
        } Else {
            // Ninguna cámara en este dispositivo
            falso retorno;
        }
    }
    Cámara estática pública getCameraInstance () {
        Cámara c = null;
        tratar {
            c = Camera.open (); // Intento de obtener una instancia de la cámara
        }
        catch (Exception e) {
            // La cámara no está disponible (en uso o no existe)
        }
        c regresar; // Devuelve null si la cámara no está disponible
    }

    CameraPreview clase pública se extiende SurfaceView implementa SurfaceHolder.Callback {
        mHolder SurfaceHolder privada;
        Cámara mCamera privada;

        CameraPreview pública (contexto Contexto, cámara de la cámara) {
            super (contexto);
            mCamera = cámara;

            // Instalar un SurfaceHolder.Callback así que ser notificado cuando el
            // Superficie subyacente se crea y se destruye.
            mHolder = getHolder ();
            mHolder.addCallback (this);
            // Desuso entorno, pero necesario en las versiones de Android anteriores a 3.0
            mHolder.setType (SurfaceHolder.SURFACE_TYPE_PUSH_BUFFERS);
        }

        public void surfaceCreated (titular SurfaceHolder) {
            // La superficie ha sido creado, ahora informar a la cámara dónde trazar la vista previa.
            tratar {
                mCamera.setDisplayOrientation (90);
                mCamera.setPreviewDisplay (titular);

                mCamera.startPreview ();
            } Catch (IOException e) {
                Log.d ( "cámara", "Error al establecer la cámara previsualización:" + e.getMessage ());
            }
        }

        public void surfaceDestroyed (titular SurfaceHolder) {
            // Vacía. Cuidar de la liberación de la vista previa de la cámara en su actividad.
        }

        surfaceChanged (titular SurfaceHolder, formato, int w, int h) {public void

            si (mHolder.getSurface () == null) {
                regreso;
            }

            tratar {
                mCamera.stopPreview ();
            } Catch (Exception e) {
            }
            tratar {
                mCamera.setPreviewDisplay (mHolder);
                mCamera.startPreview ();

            } Catch (Exception e) {
                Log.d ( "cámara", "Error al iniciar la cámara previsualización:" + e.getMessage ());
            }
        }
    }

}
