# ReproductorM

He realizado un reproductor sencillo en AndroidStudio usando Java.

El codigo Java seria el siguiente: 

import androidx.appcompat.app.AppCompatActivity;

import android.content.DialogInterface;
import android.media.AudioManager;
import android.media.MediaPlayer;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.widget.Toast;

import java.io.IOException;

public class MainActivity extends AppCompatActivity {
MediaPlayer mediaOnline;
Button play, stop, pause, btRewind, btForward;
RadioButton rbOnline, rbLocal;
EditText editTextRewind, editTextForward;
RadioGroup radioGroup;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //Aquí vemos las variables que hemos declarado
        play = (Button) findViewById(R.id.play);
        stop = (Button) findViewById(R.id.stop);
        pause = (Button) findViewById(R.id.pause);
        btRewind = (Button) findViewById(R.id.btRewind);
        btForward = (Button) findViewById(R.id.btForward);
        rbOnline = (RadioButton) findViewById(R.id.rbOnline);
        rbLocal = (RadioButton) findViewById(R.id.rbLocal);
        editTextRewind = (EditText) findViewById(R.id.editTextRewind);
        editTextForward = (EditText) findViewById(R.id.editTextForward);
        radioGroup = (RadioGroup) findViewById(R.id.radioGroup);

//Aqui escribimos el código para MediaPlayer para el archivo del audio local

        final MediaPlayer mediaLocal;
        mediaLocal = MediaPlayer.create(this, R.raw.dio_rainbow_in_the_dark);
//A Continuación he creado las clases para los botones de Play, Pause, Stop, Rewind y Forward para que realicen la actividad deseada
        //Play
        play.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (!rbOnline.isChecked() && !rbLocal.isChecked()){
                    Toast.makeText(MainActivity.this, "Elige una opción", Toast.LENGTH_SHORT).show();
                } else {
                    if (rbOnline.isChecked()){
                        mediaOnline.start();
                        Toast.makeText(MainActivity.this, "Reproduciendo Highway to Hell",
                                Toast.LENGTH_SHORT).show();
                        mediaLocal.stop();
                        try {
                            mediaLocal.prepare();
                        } catch ( IOException e) {
                            e.printStackTrace();
                        }
                    } else {
                        if (rbLocal.isChecked()){
                            mediaLocal.start();
                            Toast.makeText(MainActivity.this, "Reproduciendo Rainbow in the Dark",
                                    Toast.LENGTH_SHORT).show();
                            mediaOnline.stop();
                            try {
                                mediaOnline.prepare();
                            } catch (IOException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
            }
        });
        //Pause
        pause.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mediaOnline.isPlaying()){
                    mediaOnline.pause();
                } else {
                    if (mediaLocal.isPlaying()){
                        mediaLocal.pause();
                    }
                }
            }
        });
        //Stop
        stop.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mediaOnline.isPlaying()){
                    mediaOnline.stop();
                    try {
                        mediaOnline.prepare();
                    }catch (IOException a){
                        a.printStackTrace();
                    }
                } else {
                    if (mediaLocal.isPlaying()){
                        mediaLocal.stop();
                        try {
                            mediaLocal.prepare();
                        }catch (IOException a){
                            a.printStackTrace();
                        }
                    }
                }
            }
        });
        //Rewind
btRewind.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        if(mediaOnline.isPlaying()){
            int cogerText = Integer.parseInt(editTextRewind.getText().toString());
            int milisegundoasegundo = cogerText * 1000;
            int posRewind = mediaOnline.getCurrentPosition() - milisegundoasegundo;
            mediaOnline.seekTo(posRewind);}
        else{
            if(mediaLocal.isPlaying()){
                int cogerText = Integer.parseInt(editTextRewind.getText().toString());
                int milisegundoasegundo = cogerText * 1000;
                int posRewind = mediaLocal.getCurrentPosition() - milisegundoasegundo;
                mediaLocal.seekTo(posRewind);}
    }
    }
    });
//Forward
btForward.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        if(mediaOnline.isPlaying()){
            int cogerText = Integer.parseInt(editTextForward.getText().toString());
            int milisegundoasegundo = cogerText * 1000;
            int posRewind = mediaOnline.getCurrentPosition() + milisegundoasegundo;
            mediaOnline.seekTo(posRewind);}
        else{
            if(mediaLocal.isPlaying()){
                int cogerText = Integer.parseInt(editTextForward.getText().toString());
                int milisegundoasegundo = cogerText * 1000;
                int posRewind = mediaLocal.getCurrentPosition() + milisegundoasegundo;
                mediaLocal.seekTo(posRewind);}
        }
    }
});
//Denominamos el String Sond para usar la URL de la canción de la modalidad Online
        String Song = "https://www.ivoox.com/ac-dc-highway-to-hell_md_2668571_wp_1.mp3";
        mediaOnline = new MediaPlayer();
        mediaOnline.setAudioStreamType(AudioManager.STREAM_MUSIC);
        try {
            mediaOnline.setDataSource(Song);
        } catch (IOException e){
            e.printStackTrace();
        }
        try {
            mediaOnline.prepare();
        } catch (IOException e){
            e.printStackTrace();
        }
        //RadioGroup en los que están ambos RadioButton para elegir una canción u otra
        radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup group, int checkedId) {
                if (mediaOnline.isPlaying()){
                    mediaOnline.stop();
                    try {
                        mediaOnline.prepare();
                    } catch (IOException e){
                        e.printStackTrace();
                    }
                } else if (mediaLocal.isPlaying()){
                    mediaLocal.stop();
                    try {
                        mediaLocal.prepare();
                    }catch (IOException e){
                        e.printStackTrace();
                    }
                }
            }
        });
    }
}


