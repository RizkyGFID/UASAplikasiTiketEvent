# Aplikasi Pembelian Tiket Event

Berikut ada beberapa isi dari kode Java dan XML untuk Aplikasi Pembelian Tiket Event Ini

# Java

1. EventDetailActivity.java

```
package com.example.ticket;

import android.content.Intent; // Jangan lupa import ini
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class EventDetailActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_event_detail);

        Button btnBuyTicket = findViewById(R.id.btnBuyTicket);

        
        btnBuyTicket.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                
                Intent intent = new Intent(EventDetailActivity.this, TicketSelectionActivity.class);
                startActivity(intent);
            }
        });
    }
}

```

2. HomeActivity.java

```
package com.example.ticket;

import android.content.DialogInterface;
import android.content.Intent;
import android.content.SharedPreferences; 
import android.os.Bundle;
import android.text.InputType; 
import android.view.View;
import android.widget.EditText; 
import android.widget.TextView;
import android.widget.Toast;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
import androidx.cardview.widget.CardView;

public class HomeActivity extends AppCompatActivity {

    TextView tvWelcome;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_home);

        tvWelcome = findViewById(R.id.tvWelcome); 
        CardView menuInfo = findViewById(R.id.menuInfo);
        CardView menuLogout = findViewById(R.id.menuLogout);

        CardView menuInfoApp = findViewById(R.id.menuInfoApp);

        CardView cardBanner = findViewById(R.id.cardBanner);
        CardView menuMyTicket = findViewById(R.id.menuMyTicket);

        SharedPreferences prefs = getSharedPreferences("UserPrefs", MODE_PRIVATE);
        String namaSimpanan = prefs.getString("nama_user", "User"); // Default "User"
        tvWelcome.setText("Halo, " + namaSimpanan + "!");

        tvWelcome.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                munculkanDialogGantiNama();
            }
        });


        if (cardBanner != null) {
            cardBanner.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Intent intent = new Intent(HomeActivity.this, EventDetailActivity.class);
                    startActivity(intent);
                }
            });
        }

        if (menuMyTicket != null) {
            menuMyTicket.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Intent intent = new Intent(HomeActivity.this, MyTicketActivity.class);
                    startActivity(intent);
                }
            });
        }

        if (menuInfo != null) {
            menuInfo.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Intent intent = new Intent(HomeActivity.this, LocationActivity.class);
                    startActivity(intent);
                }
            });
        }

        if (menuInfoApp != null) {
            menuInfoApp.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Intent intent = new Intent(HomeActivity.this, InfoActivity.class);
                    startActivity(intent);
                }
            });
        }


        if (menuLogout != null) {
            menuLogout.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Intent intent = new Intent(HomeActivity.this, LoginActivity.class);
                    intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
                    startActivity(intent);
                    finish();
                    Toast.makeText(HomeActivity.this, "Berhasil Keluar", Toast.LENGTH_SHORT).show();
                }
            });
        }
    }

    private void munculkanDialogGantiNama() {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("Ganti Nama Panggilan");

        final EditText input = new EditText(this);
        input.setInputType(InputType.TYPE_CLASS_TEXT);
        input.setHint("Masukkan nama baru...");
        builder.setView(input);

        builder.setPositiveButton("Simpan", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                String namaBaru = input.getText().toString();

                if (!namaBaru.isEmpty()) {
                    tvWelcome.setText("Halo, " + namaBaru + "!");

                    SharedPreferences prefs = getSharedPreferences("UserPrefs", MODE_PRIVATE);
                    SharedPreferences.Editor editor = prefs.edit();
                    editor.putString("nama_user", namaBaru);
                    editor.apply();

                    Toast.makeText(HomeActivity.this, "Nama berhasil diganti!", Toast.LENGTH_SHORT).show();
                }
            }
        });

        builder.setNegativeButton("Batal", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                dialog.cancel();
            }
        });

        builder.show();
    }
}

```

3. InfoActivity.java

```
package com.example.ticket;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class InfoActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_info);
    }
}

```

4. LanguageActivity.java

```
package com.example.ticket;

import android.content.Intent;
import android.content.res.Configuration;
import android.content.res.Resources;
import android.os.Bundle;
import android.util.DisplayMetrics;
import android.view.View;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;
import androidx.cardview.widget.CardView;

import java.util.Locale;

public class LanguageActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_language);

        CardView cardIndo = findViewById(R.id.cardIndo);
        CardView cardEnglish = findViewById(R.id.cardEnglish);

        CardView cardArabic = findViewById(R.id.cardArabic);
        CardView cardJapanese = findViewById(R.id.cardJapanese);

        // 1. INDONESIA
        cardIndo.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                setAppLocale("in");
                pindahKeLogin();
            }
        });

        // 2. INGGRIS
        cardEnglish.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                setAppLocale("en");
                pindahKeLogin();
            }
        });

        // 3. ARAB
        cardArabic.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                setAppLocale("ar"); // Kode bahasa Arab
                pindahKeLogin();
            }
        });

        // 4. JEPANG
        cardJapanese.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                setAppLocale("ja"); // Kode bahasa Jepang
                pindahKeLogin();
            }
        });
    }

    private void setAppLocale(String localeCode) {
        Resources res = getResources();
        DisplayMetrics dm = res.getDisplayMetrics();
        Configuration conf = res.getConfiguration();
        conf.setLocale(new Locale(localeCode.toLowerCase()));
        res.updateConfiguration(conf, dm);

        String msg = "Bahasa diubah: " + localeCode;
        if(localeCode.equals("ar")) msg = "اللغة المختارة: العربية";
        if(localeCode.equals("ja")) msg = "言語が変更されました";

        Toast.makeText(this, msg, Toast.LENGTH_SHORT).show();
    }

    private void pindahKeLogin() {
        Intent intent = new Intent(LanguageActivity.this, LoginActivity.class);
        startActivity(intent);
        finish();
    }
}

```

5. LocationActivity.java

```
package com.example.ticket;

import android.Manifest;
import android.content.Context;
import android.content.SharedPreferences;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.preference.PreferenceManager;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

import org.osmdroid.config.Configuration;
import org.osmdroid.tileprovider.tilesource.TileSourceFactory;
import org.osmdroid.util.GeoPoint;
import org.osmdroid.views.MapView;
import org.osmdroid.views.overlay.mylocation.GpsMyLocationProvider;
import org.osmdroid.views.overlay.mylocation.MyLocationNewOverlay;

public class LocationActivity extends AppCompatActivity {

    private MapView map;
    private MyLocationNewOverlay locationOverlay; // Overlay untuk titik biru lokasi user
    private static final int REQUEST_CODE_LOCATION = 1; 

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Context ctx = getApplicationContext();
        Configuration.getInstance().load(ctx, PreferenceManager.getDefaultSharedPreferences(ctx));

        setContentView(R.layout.activity_location);

        map = findViewById(R.id.map);
        map.setTileSource(TileSourceFactory.MAPNIK);
        map.setMultiTouchControls(true);
        map.getController().setZoom(18.0);

        GeoPoint startPoint = new GeoPoint(-6.175392, 106.827153);
        map.getController().setCenter(startPoint);

        checkLocationPermission();

        Button btnCenter = findViewById(R.id.btnCenter);
        btnCenter.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (locationOverlay != null && locationOverlay.getMyLocation() != null) {
                    map.getController().animateTo(locationOverlay.getMyLocation());
                } else {
                    Toast.makeText(LocationActivity.this, "Sedang mencari lokasi...", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }


    private void checkLocationPermission() {
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, REQUEST_CODE_LOCATION);
        } else {
            aktifkanLokasiUser();
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == REQUEST_CODE_LOCATION) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                Toast.makeText(this, "Izin diberikan, menampilkan lokasi...", Toast.LENGTH_SHORT).show();
                aktifkanLokasiUser();
            } else {
                Toast.makeText(this, "Izin ditolak. Peta hanya menampilkan lokasi default.", Toast.LENGTH_LONG).show();
            }
        }
    }

    private void aktifkanLokasiUser() {
        locationOverlay = new MyLocationNewOverlay(new GpsMyLocationProvider(this), map);
        locationOverlay.enableMyLocation();
        locationOverlay.enableFollowLocation(); 
        map.getOverlays().add(locationOverlay);

        map.invalidate();
    }

    @Override
    public void onResume() {
        super.onResume();
        if (map != null) map.onResume();
        if (locationOverlay != null) locationOverlay.enableMyLocation();
    }

    @Override
    public void onPause() {
        super.onPause();
        if (map != null) map.onPause();
        if (locationOverlay != null) locationOverlay.disableMyLocation();
    }
}

```

6. LoginActivity.java

```
package com.example.ticket;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class LoginActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        EditText etUsername = findViewById(R.id.etUsername);
        EditText etPassword = findViewById(R.id.etPassword);
        Button btnLogin = findViewById(R.id.btnLogin);

        btnLogin.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String username = etUsername.getText().toString();
                String password = etPassword.getText().toString();

                if (username.isEmpty() || password.isEmpty()) {
                    Toast.makeText(LoginActivity.this, "Harap isi Username dan Password", Toast.LENGTH_SHORT).show();
                } else {
                    Intent intent = new Intent(LoginActivity.this, HomeActivity.class);
                    startActivity(intent);
                    finish(); 
                }
            }
        });
    }
}

```

7. MainActivity.java

```
package com.example.ticket;

import android.content.Intent;
import android.os.Bundle;
import android.os.Handler;
import android.os.Looper;
import android.view.animation.Animation;
import android.view.animation.AnimationUtils;
import android.widget.ImageView;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ImageView imgLogo = findViewById(R.id.imageView);
        TextView tvTitle = findViewById(R.id.textView2);

        Animation animBottom = AnimationUtils.loadAnimation(this, R.anim.bottom_to_top);
        Animation animFade = AnimationUtils.loadAnimation(this, R.anim.fade_in);

        imgLogo.startAnimation(animBottom); 
        tvTitle.startAnimation(animFade); 

        new Handler(Looper.getMainLooper()).postDelayed(new Runnable() {
            @Override
            public void run() {
                Intent intent = new Intent(MainActivity.this, LanguageActivity.class);
                startActivity(intent);
                finish();
            }
        }, 3000);
    }
}

```

8. MyTicketActivity.java

```
package com.example.ticket;

import android.graphics.Bitmap;
import android.os.Bundle;
import android.view.View;
import android.widget.ImageView;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;
import androidx.cardview.widget.CardView;

import com.google.zxing.BarcodeFormat;
import com.google.zxing.MultiFormatWriter;
import com.google.zxing.WriterException;
import com.google.zxing.common.BitMatrix;
import com.journeyapps.barcodescanner.BarcodeEncoder;

public class MyTicketActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_my_ticket);

        CardView cardTicket = findViewById(R.id.cardTicket);
        TextView tvNoTicket = findViewById(R.id.tvNoTicket);
        TextView tvTicketType = findViewById(R.id.tvTicketType);
        ImageView imgTicketQR = findViewById(R.id.imgTicketQR);

        boolean userSudahBeli = true; 
        String jenisTiket = "VIP (ALL ACCESS)";
        
        if (userSudahBeli) {
            cardTicket.setVisibility(View.VISIBLE);
            tvNoTicket.setVisibility(View.GONE);
            tvTicketType.setText(jenisTiket);

            String contentQR = "TICKET_VALID|2025|" + jenisTiket + "|USER_ID_001";

            try {
                MultiFormatWriter multiFormatWriter = new MultiFormatWriter();
                BitMatrix bitMatrix = multiFormatWriter.encode(contentQR, BarcodeFormat.QR_CODE, 400, 400);
                BarcodeEncoder barcodeEncoder = new BarcodeEncoder();
                Bitmap bitmap = barcodeEncoder.createBitmap(bitMatrix);

                imgTicketQR.setImageBitmap(bitmap);

            } catch (WriterException e) {
                e.printStackTrace();
            }

        } else {
            cardTicket.setVisibility(View.GONE);
            tvNoTicket.setVisibility(View.VISIBLE);
        }
    }
}

```

9. PaymentActivity.java

```
package com.example.ticket;

import android.content.Intent;
import android.graphics.Bitmap;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.TextView;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import com.google.zxing.BarcodeFormat; 
import com.google.zxing.MultiFormatWriter;
import com.google.zxing.WriterException;
import com.google.zxing.common.BitMatrix;
import com.journeyapps.barcodescanner.BarcodeEncoder; 

public class PaymentActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_payment);

        String ticketName = getIntent().getStringExtra("TICKET_NAME");
        String ticketPrice = getIntent().getStringExtra("TICKET_PRICE");

        if (ticketName == null) ticketName = "Tiket Demo";
        if (ticketPrice == null) ticketPrice = "Rp 0";

        TextView tvTicketName = findViewById(R.id.tvTicketName);
        TextView tvTicketPrice = findViewById(R.id.tvTicketPrice);
        TextView tvTotalPrice = findViewById(R.id.tvTotalPrice);
        ImageView imgQRCode = findViewById(R.id.imgQRCode); // Gambar QR
        Button btnPayNow = findViewById(R.id.btnPayNow);

        tvTicketName.setText(ticketName);
        tvTicketPrice.setText(ticketPrice);
        tvTotalPrice.setText(ticketPrice);

        String contentQR = "PAYMENT_ID:12345|" + ticketName + "|" + ticketPrice;

        try {
            MultiFormatWriter multiFormatWriter = new MultiFormatWriter();
            BitMatrix bitMatrix = multiFormatWriter.encode(contentQR, BarcodeFormat.QR_CODE, 500, 500);

            BarcodeEncoder barcodeEncoder = new BarcodeEncoder();
            Bitmap bitmap = barcodeEncoder.createBitmap(bitMatrix);

            imgQRCode.setImageBitmap(bitmap);

        } catch (WriterException e) {
            e.printStackTrace();
        }

        btnPayNow.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(PaymentActivity.this, "Pembayaran Berhasil!", Toast.LENGTH_LONG).show();

                Intent intent = new Intent(PaymentActivity.this, HomeActivity.class);
                intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP | Intent.FLAG_ACTIVITY_NEW_TASK);
                startActivity(intent);
                finish();
            }
        });
    }
}

```

10. PermissionActivity.java

```
package com.example.ticket;

import android.Manifest;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

public class PermissionActivity extends AppCompatActivity {

    private static final int LOCATION_PERMISSION_CODE = 100;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_permission);

        Button btnAllow = findViewById(R.id.btnAllow);
        TextView btnSkip = findViewById(R.id.btnSkip);

        if (checkPermission()) {
            goToLogin();
        }

        btnAllow.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                requestPermission();
            }
        });

        btnSkip.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                goToLogin(); 
            }
        });
    }

    private boolean checkPermission() {
        return ContextCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) == PackageManager.PERMISSION_GRANTED;
    }

    private void requestPermission() {
        ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, LOCATION_PERMISSION_CODE);
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == LOCATION_PERMISSION_CODE) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                Toast.makeText(this, "Izin Lokasi Diberikan", Toast.LENGTH_SHORT).show();
                goToLogin();
            } else {
                Toast.makeText(this, "Izin Ditolak", Toast.LENGTH_SHORT).show();
                goToLogin();
            }
        }
    }

    private void goToLogin() {
        Intent intent = new Intent(PermissionActivity.this, LoginActivity.class);
        startActivity(intent);
        finish();
    }
}

```

11. TicketSelectionActivity.java

```
package com.example.ticket;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class TicketSelectionActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_ticket_selection);

        Button btnRegular = findViewById(R.id.btnRegular);
        Button btnRegular4Hari = findViewById(R.id.btnRegular4Hari);
        Button btnVvip = findViewById(R.id.btnVvip);

        btnRegular.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                pindahKePembayaran("Tiket Regular", "Rp 70.000");
            }
        });

        btnRegular4Hari.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                pindahKePembayaran("Tiket Regular 4 Hari", "Rp 230.000");
            }
        });

        btnVvip.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                pindahKePembayaran("Tiket VIP (All Access)", "Rp 500.000");
            }
        });
    }

    private void pindahKePembayaran(String namaTiket, String hargaTiket) {
        Intent intent = new Intent(TicketSelectionActivity.this, PaymentActivity.class);

        intent.putExtra("TICKET_NAME", namaTiket);
        intent.putExtra("TICKET_PRICE", hargaTiket);

        startActivity(intent);
    }
}

```

# XML

1. activity_event_detail.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    tools:context=".EventDetailActivity">

    <!-- Area Konten (Bisa di-scroll) -->
    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toTopOf="@id/layoutButton">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">

            <!-- Gambar Banner Besar -->
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="250dp"
                android:scaleType="centerCrop"
                android:src="@drawable/hoyofest" />

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="20dp">

                <!-- Judul Event -->
                <TextView
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:text="@string/event_title"
                    android:textSize="24sp"
                    android:textStyle="bold"
                    android:textColor="#2196F3"
                    android:layout_marginBottom="8dp"/>

                <!-- Lokasi -->
                <TextView
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:text="@string/event_location"
                    android:textSize="14sp"
                    android:textColor="#666666"
                    android:layout_marginBottom="20dp"/>

                <!-- Judul Deskripsi -->
                <TextView
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:text="@string/event_desc_title"
                    android:textStyle="bold"
                    android:textSize="16sp"
                    android:layout_marginBottom="8dp"/>

                <!-- Isi Deskripsi -->
                <TextView
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:text="@string/event_desc_content"
                    android:textSize="14sp"
                    android:lineSpacingExtra="4dp"
                    android:textColor="#333333"/>
            </LinearLayout>
        </LinearLayout>
    </ScrollView>

    <!-- Area Tombol Beli (Tetap di Bawah) -->
    <LinearLayout
        android:id="@+id/layoutButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="16dp"
        android:background="#FFFFFF"
        android:elevation="10dp"
        app:layout_constraintBottom_toBottomOf="parent">

        <Button
            android:id="@+id/btnBuyTicket"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/btn_buy_ticket"
            android:textSize="16sp"
            android:textStyle="bold"
            android:backgroundTint="#FF5722"
            android:padding="14dp"/>
    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>

```

2. activity_home.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F5F5F5"
    tools:context=".HomeActivity">

    <!-- 1. Header Biru -->
    <View
        android:id="@+id/viewHeader"
        android:layout_width="match_parent"
        android:layout_height="220dp"
        android:background="#2196F3"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/tvWelcome"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/home_greeting"
        android:textSize="26sp"
        android:textStyle="bold"
        android:textColor="#FFFFFF"
        android:layout_marginStart="24dp"
        android:layout_marginTop="40dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/home_subtitle"
        android:textColor="#E0E0E0"
        android:textSize="14sp"
        android:layout_marginTop="5dp"
        app:layout_constraintTop_toBottomOf="@id/tvWelcome"
        app:layout_constraintStart_toStartOf="@id/tvWelcome" />

    <!-- 2. CardView Banner -->
    <androidx.cardview.widget.CardView
        android:id="@+id/cardBanner"
        android:layout_width="match_parent"
        android:layout_height="160dp"
        android:layout_marginHorizontal="20dp"
        android:layout_marginTop="30dp"
        app:cardCornerRadius="16dp"
        app:cardElevation="8dp"
        android:clickable="true"
        android:focusable="true"
        android:foreground="?android:attr/selectableItemBackground"
        app:layout_constraintTop_toBottomOf="@id/tvWelcome"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent">

        <androidx.constraintlayout.widget.ConstraintLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scaleType="centerCrop"
                android:src="@drawable/hoyofest"
                android:alpha="0.3"
                android:background="#EEEEEE"/>

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/event_title"
                android:textColor="#2196F3"
                android:textSize="25sp"
                android:textStyle="bold"
                app:layout_constraintBottom_toBottomOf="parent"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toTopOf="parent"
                app:layout_constraintEnd_toEndOf="parent"/>

        </androidx.constraintlayout.widget.ConstraintLayout>
    </androidx.cardview.widget.CardView>

    <!-- 3. Menu Pilihan -->
    <TextView
        android:id="@+id/tvMenuLabel"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/menu_label"
        android:textSize="18sp"
        android:textStyle="bold"
        android:textColor="#333333"
        android:layout_marginTop="25dp"
        android:layout_marginStart="24dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/cardBanner" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:paddingHorizontal="20dp"
        android:layout_marginTop="15dp"
        app:layout_constraintTop_toBottomOf="@id/tvMenuLabel">

        <!-- Baris 1 -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:layout_marginBottom="15dp"
            android:weightSum="2">

            <!-- Menu 1: LOKASI -->
            <androidx.cardview.widget.CardView
                android:id="@+id/menuInfo"
                android:layout_width="0dp"
                android:layout_height="120dp"
                android:layout_weight="1"
                android:layout_marginEnd="8dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="4dp"
                android:clickable="true"
                android:foreground="?android:attr/selectableItemBackground">
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:orientation="vertical"
                    android:gravity="center">
                    <ImageView
                        android:layout_width="50dp"
                        android:layout_height="50dp"
                        android:src="@android:drawable/ic_dialog_map"
                        app:tint="#2196F3"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/menu_location"
                        android:textStyle="bold"
                        android:layout_marginTop="8dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>

            <!-- Menu 2: TIKET SAYA -->
            <androidx.cardview.widget.CardView
                android:id="@+id/menuMyTicket"
                android:layout_width="0dp"
                android:layout_height="120dp"
                android:layout_weight="1"
                android:layout_marginStart="8dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="4dp"
                android:clickable="true"
                android:foreground="?android:attr/selectableItemBackground">
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:orientation="vertical"
                    android:gravity="center">
                    <ImageView
                        android:layout_width="50dp"
                        android:layout_height="50dp"
                        android:src="@android:drawable/ic_dialog_email"
                        app:tint="#FF9800"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/menu_my_ticket"
                        android:textStyle="bold"
                        android:layout_marginTop="8dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>
        </LinearLayout>

        <!-- Baris 2 -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:weightSum="2">

            <!-- Menu 3: INFORMASI -->
            <androidx.cardview.widget.CardView
                android:id="@+id/menuInfoApp"
                android:layout_width="0dp"
                android:layout_height="120dp"
                android:layout_weight="1"
                android:layout_marginEnd="8dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="4dp"
                android:clickable="true"
                android:foreground="?android:attr/selectableItemBackground">
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:orientation="vertical"
                    android:gravity="center">
                    <ImageView
                        android:layout_width="50dp"
                        android:layout_height="50dp"
                        android:src="@android:drawable/ic_menu_info_details"
                        app:tint="#4CAF50"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/menu_info"
                        android:textStyle="bold"
                        android:layout_marginTop="8dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>

            <!-- Menu 4: KELUAR -->
            <androidx.cardview.widget.CardView
                android:id="@+id/menuLogout"
                android:layout_width="0dp"
                android:layout_height="120dp"
                android:layout_weight="1"
                android:layout_marginStart="8dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="4dp"
                android:clickable="true"
                android:foreground="?android:attr/selectableItemBackground">
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:orientation="vertical"
                    android:gravity="center">
                    <ImageView
                        android:layout_width="50dp"
                        android:layout_height="50dp"
                        android:src="@android:drawable/ic_lock_power_off"
                        app:tint="#F44336"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/menu_logout"
                        android:textStyle="bold"
                        android:layout_marginTop="8dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>
        </LinearLayout>
    </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>

```

3. activity_info.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    android:padding="24dp"
    tools:context=".InfoActivity">

    <!-- Logo (Pastikan gambar logo ada di drawable) -->
    <ImageView
        android:id="@+id/imgLogo"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:src="@android:drawable/ic_menu_info_details"
        android:layout_marginTop="60dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <!-- Judul Aplikasi -->
    <TextView
        android:id="@+id/tvAppName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/app_name"
        android:textSize="28sp"
        android:textStyle="bold"
        android:textColor="#2196F3"
        android:layout_marginTop="16dp"
        app:layout_constraintTop_toBottomOf="@id/imgLogo"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <!-- Versi Aplikasi -->
    <TextView
        android:id="@+id/tvVersion"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/app_version"
        android:textColor="#999999"
        android:layout_marginTop="4dp"
        app:layout_constraintTop_toBottomOf="@id/tvAppName"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <!-- Deskripsi Aplikasi -->
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/app_description"
        android:textSize="16sp"
        android:textColor="#333333"
        android:gravity="center"
        android:lineSpacingExtra="6dp"
        android:layout_marginTop="32dp"
        app:layout_constraintTop_toBottomOf="@id/tvVersion"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <!-- Copyright -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/copyright"
        android:textSize="12sp"
        android:textColor="#BBBBBB"
        android:layout_marginBottom="24dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>

```

4. activity_language.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    tools:context=".LanguageActivity">

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:paddingBottom="40dp">

            <!-- Judul -->
            <TextView
                android:id="@+id/tvTitleLang"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="Select Language\nPilih Bahasa"
                android:textSize="24sp"
                android:textStyle="bold"
                android:textColor="#333333"
                android:textAlignment="center"
                android:layout_marginTop="60dp"
                android:layout_marginBottom="30dp"/>

            <!-- Pilihan 1: Indonesia -->
            <androidx.cardview.widget.CardView
                android:id="@+id/cardIndo"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginHorizontal="30dp"
                android:layout_marginBottom="20dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="6dp"
                android:clickable="true"
                android:foreground="?android:attr/selectableItemBackground">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="horizontal"
                    android:padding="20dp"
                    android:gravity="center_vertical">
                    <ImageView
                        android:layout_width="40dp"
                        android:layout_height="40dp"
                        android:src="@android:drawable/ic_menu_myplaces"
                        app:tint="#F44336"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Bahasa Indonesia"
                        android:textSize="18sp"
                        android:textStyle="bold"
                        android:layout_marginStart="20dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>

            <!-- Pilihan 2: English -->
            <androidx.cardview.widget.CardView
                android:id="@+id/cardEnglish"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginHorizontal="30dp"
                android:layout_marginBottom="20dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="6dp"
                android:clickable="true"
                android:foreground="?android:attr/selectableItemBackground">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="horizontal"
                    android:padding="20dp"
                    android:gravity="center_vertical">
                    <ImageView
                        android:layout_width="40dp"
                        android:layout_height="40dp"
                        android:src="@android:drawable/ic_menu_myplaces"
                        app:tint="#2196F3"/>
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="English"
                        android:textSize="18sp"
                        android:textStyle="bold"
                        android:layout_marginStart="20dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>

            <!-- Pilihan 3: Arab (BARU) -->
            <androidx.cardview.widget.CardView
                android:id="@+id/cardArabic"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginHorizontal="30dp"
                android:layout_marginBottom="20dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="6dp"
                android:clickable="true"
                android:foreground="?android:attr/selectableItemBackground">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="horizontal"
                    android:padding="20dp"
                    android:gravity="center_vertical">
                    <ImageView
                        android:layout_width="40dp"
                        android:layout_height="40dp"
                        android:src="@android:drawable/ic_menu_myplaces"
                        app:tint="#009688"/> <!-- Warna Hijau -->
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="العربية (Arabic)"
                        android:textSize="18sp"
                        android:textStyle="bold"
                        android:layout_marginStart="20dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>

            <!-- Pilihan 4: Jepang (BARU) -->
            <androidx.cardview.widget.CardView
                android:id="@+id/cardJapanese"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginHorizontal="30dp"
                android:layout_marginBottom="20dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="6dp"
                android:clickable="true"
                android:foreground="?android:attr/selectableItemBackground">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="horizontal"
                    android:padding="20dp"
                    android:gravity="center_vertical">
                    <ImageView
                        android:layout_width="40dp"
                        android:layout_height="40dp"
                        android:src="@android:drawable/ic_menu_myplaces"
                        app:tint="#E91E63"/> <!-- Warna Pink -->
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="日本語 (Japanese)"
                        android:textSize="18sp"
                        android:textStyle="bold"
                        android:layout_marginStart="20dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>

        </LinearLayout>
    </ScrollView>

</androidx.constraintlayout.widget.ConstraintLayout>

```

5. activity_location.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".LocationActivity">

    <!-- Logo -->
    <ImageView
        android:id="@+id/imageView3"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:layout_marginTop="24dp"
        app:srcCompat="@drawable/logo_aplikasi_ticket_non_background"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Judul Halaman -->
    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/location_title"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginTop="16dp"
        app:layout_constraintTop_toBottomOf="@id/imageView3"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <!-- Peta -->
    <org.osmdroid.views.MapView
        android:id="@+id/map"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:layout_margin="20dp"
        app:layout_constraintTop_toBottomOf="@id/tvTitle"
        app:layout_constraintBottom_toTopOf="@id/btnCenter"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Tombol untuk Reset Lokasi -->
    <Button
        android:id="@+id/btnCenter"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/btn_see_location"
        android:layout_marginBottom="32dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>

```

6. activity_login.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    tools:context=".LoginActivity">

    <!-- Logo Aplikasi -->
    <ImageView
        android:id="@+id/logoLogin"
        android:layout_width="120dp"
        android:layout_height="120dp"
        android:layout_marginTop="60dp"
        app:srcCompat="@drawable/logo_aplikasi_ticket_non_background"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <!-- Judul Halaman (Gunakan welcome_message) -->
    <TextView
        android:id="@+id/tvLoginTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/welcome_message"
        android:textSize="28sp"
        android:textStyle="bold"
        android:textColor="#000000"
        android:layout_marginTop="20dp"
        app:layout_constraintTop_toBottomOf="@id/logoLogin"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Kolom Input Email/Username -->
    <EditText
        android:id="@+id/etUsername"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="@string/username"
        android:inputType="textEmailAddress"
        android:minHeight="48dp"
        android:background="@android:drawable/edit_text"
        android:layout_marginHorizontal="32dp"
        android:layout_marginTop="40dp"
        app:layout_constraintTop_toBottomOf="@id/tvLoginTitle"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Kolom Input Password -->
    <EditText
        android:id="@+id/etPassword"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="@string/password"
        android:inputType="textPassword"
        android:minHeight="48dp"
        android:background="@android:drawable/edit_text"
        android:layout_marginHorizontal="32dp"
        android:layout_marginTop="20dp"
        app:layout_constraintTop_toBottomOf="@id/etUsername"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Tombol Login -->
    <Button
        android:id="@+id/btnLogin"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="@string/btn_login"
        android:textStyle="bold"
        android:textSize="16sp"
        android:layout_marginHorizontal="32dp"
        android:layout_marginTop="40dp"
        app:layout_constraintTop_toBottomOf="@id/etPassword"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Teks Daftar (Biarkan manual dulu karena belum ada di strings.xml Anda) -->
    <TextView
        android:id="@+id/tvRegister"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Belum punya akun? Daftar disini"
        android:textColor="#555555"
        android:layout_marginTop="20dp"
        app:layout_constraintTop_toBottomOf="@id/btnLogin"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>

```

7. activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="173dp"
        android:layout_height="435dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.4"
        app:srcCompat="@drawable/logo_aplikasi_ticket_non_background" />

    <!-- Perhatikan baris di atas ^^^ saya ubah jadi layout_constraintVertical_bias -->

    <TextView
        android:id="@+id/textView2"
        android:layout_width="191dp"
        android:layout_height="71dp"
        android:layout_marginTop="4dp"
        android:fontFamily="@font/irish_groover"
        android:text="TICKET EVENT AREA"
        android:textAlignment="center"
        android:textColor="#000000"
        android:textSize="28sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.496"
        app:layout_constraintVertical_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/imageView" />

</androidx.constraintlayout.widget.ConstraintLayout>

```

8. activity_my_ticket.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#2196F3"
    tools:context=".MyTicketActivity">

    <!-- Judul Halaman -->
    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/my_ticket_title"
        android:textSize="24sp"
        android:textStyle="bold"
        android:textColor="#FFFFFF"
        android:layout_marginTop="40dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <!-- KARTU TIKET -->
    <androidx.cardview.widget.CardView
        android:id="@+id/cardTicket"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="30dp"
        app:cardCornerRadius="16dp"
        app:cardElevation="10dp"
        app:layout_constraintTop_toBottomOf="@id/tvTitle"
        app:layout_constraintBottom_toBottomOf="parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="0dp">

            <!-- Header Tiket (Gambar Banner) -->
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="120dp"
                android:src="@drawable/hoyofest"
                android:scaleType="centerCrop"/>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="vertical"
                android:padding="24dp">

                <!-- Judul Event -->
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/event_title"
                    android:textSize="20sp"
                    android:textStyle="bold"
                    android:textColor="#333333"/>

                <!-- Jenis Tiket (Masih manual karena data dinamis dari DB/Intent, tapi labelnya bisa dilokalkan jika perlu) -->
                <TextView
                    android:id="@+id/tvTicketType"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/ticket_vip"
                    android:textSize="18sp"
                    android:textColor="#FF9800"
                    android:textStyle="bold"
                    android:layout_marginTop="8dp"/>

                <View
                    android:layout_width="match_parent"
                    android:layout_height="2dp"
                    android:background="#CCCCCC"
                    android:layout_marginVertical="16dp"
                    android:layerType="software" />

                <!-- Area QR Code -->
                <ImageView
                    android:id="@+id/imgTicketQR"
                    android:layout_width="150dp"
                    android:layout_height="150dp"
                    android:layout_gravity="center"
                    android:scaleType="fitCenter"
                    android:layout_marginTop="10dp"
                    android:src="@android:drawable/ic_menu_gallery" />

                <!-- Instruksi QR -->
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/show_qr_instruction"
                    android:gravity="center"
                    android:layout_gravity="center"
                    android:layout_marginTop="10dp"
                    android:textSize="12sp"/>

                <!-- Status Pembayaran -->
                <TextView
                    android:id="@+id/tvStatus"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/status_paid"
                    android:textColor="#4CAF50"
                    android:textStyle="bold"
                    android:layout_gravity="center"
                    android:layout_marginTop="16dp"
                    android:textSize="16sp"/>

            </LinearLayout>
        </LinearLayout>
    </androidx.cardview.widget.CardView>

    <!-- Tampilan Kalau Belum Beli (Default Hidden) -->
    <TextView
        android:id="@+id/tvNoTicket"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/no_ticket"
        android:textColor="#FFFFFF"
        android:textSize="18sp"
        android:visibility="gone"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>

```

9. activity_payment.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F5F5F5"
    android:padding="20dp"
    tools:context=".PaymentActivity">

    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/payment_title"
        android:textSize="24sp"
        android:textStyle="bold"
        android:textColor="#333333"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"/>

    <!-- KARTU RINGKASAN PESANAN -->
    <androidx.cardview.widget.CardView
        android:id="@+id/cardSummary"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp"
        app:cardCornerRadius="12dp"
        app:cardElevation="4dp"
        app:layout_constraintTop_toBottomOf="@id/tvTitle">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="20dp">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/order_summary"
                android:textStyle="bold"
                android:textSize="18sp"
                android:layout_marginBottom="10dp"/>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="horizontal"
                android:layout_marginBottom="5dp">
                <TextView
                    android:id="@+id/tvTicketName"
                    android:layout_width="0dp"
                    android:layout_weight="1"
                    android:layout_height="wrap_content"
                    android:text="@string/ticket_regular"
                    android:textSize="16sp"/>
                <TextView
                    android:id="@+id/tvTicketPrice"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Rp 70.000"
                    android:textStyle="bold"
                    android:textSize="16sp"/>
            </LinearLayout>

            <View
                android:layout_width="match_parent"
                android:layout_height="1dp"
                android:background="#DDDDDD"
                android:layout_marginVertical="10dp"/>

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="horizontal">
                <TextView
                    android:layout_width="0dp"
                    android:layout_weight="1"
                    android:layout_height="wrap_content"
                    android:text="@string/total_pay"
                    android:textStyle="bold"
                    android:textSize="18sp"/>
                <TextView
                    android:id="@+id/tvTotalPrice"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="Rp 70.000"
                    android:textColor="#4CAF50"
                    android:textStyle="bold"
                    android:textSize="18sp"/>
            </LinearLayout>
        </LinearLayout>
    </androidx.cardview.widget.CardView>

    <!-- METODE PEMBAYARAN -->
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/payment_method"
        android:textSize="16sp"
        android:textStyle="bold"
        android:layout_marginTop="20dp"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/cardSummary"/>

    <!-- AREA TAMPIL QR CODE GENERATE -->
    <androidx.cardview.widget.CardView
        android:id="@+id/cardQRIS"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:cardCornerRadius="12dp"
        app:cardElevation="4dp"
        app:layout_constraintTop_toBottomOf="@+id/cardSummary"
        android:layout_marginTop="20dp"
        android:layout_marginBottom="80dp">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="20dp"
            android:gravity="center">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/scan_qris"
                android:textStyle="bold"
                android:textSize="16sp"
                android:layout_marginBottom="15dp"/>

            <!-- INI IMAGEVIEW UNTUK QR CODE HASIL GENERATE -->
            <ImageView
                android:id="@+id/imgQRCode"
                android:layout_width="200dp"
                android:layout_height="200dp"
                android:src="@android:drawable/ic_menu_camera"
                android:scaleType="fitCenter"/>

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/auto_generated"
                android:textSize="12sp"
                android:textColor="#888888"
                android:layout_marginTop="10dp"/>
        </LinearLayout>
    </androidx.cardview.widget.CardView>


    <!-- TOMBOL BAYAR -->
    <Button
        android:id="@+id/btnPayNow"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/btn_pay_now"
        android:textStyle="bold"
        android:padding="15dp"
        android:backgroundTint="#2196F3"
        app:layout_constraintBottom_toBottomOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>

```

10. activity_permission.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    android:padding="24dp"
    tools:context=".PermissionActivity">

    <!-- Ikon Lokasi Besar -->
    <ImageView
        android:id="@+id/imgLocation"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:layout_marginTop="80dp"
        android:src="@android:drawable/ic_dialog_map"
        app:tint="#2196F3"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Judul -->
    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Izin Lokasi Diperlukan"
        android:textSize="22sp"
        android:textStyle="bold"
        android:textColor="#000000"
        android:layout_marginTop="24dp"
        app:layout_constraintTop_toBottomOf="@id/imgLocation"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Penjelasan -->
    <TextView
        android:id="@+id/tvDesc"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Aplikasi ini memerlukan akses lokasi Anda untuk menampilkan peta dan event di sekitar Anda dengan akurat."
        android:textAlignment="center"
        android:textSize="16sp"
        android:textColor="#666666"
        android:layout_marginTop="16dp"
        app:layout_constraintTop_toBottomOf="@id/tvTitle"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Tombol Izinkan -->
    <Button
        android:id="@+id/btnAllow"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="IZINKAN LOKASI"
        android:textStyle="bold"
        android:padding="12dp"
        android:layout_marginBottom="16dp"
        app:layout_constraintBottom_toTopOf="@id/btnSkip"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <!-- Tombol Lewati/Nanti Saja -->
    <TextView
        android:id="@+id/btnSkip"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Nanti Saja"
        android:textColor="#999999"
        android:textSize="16sp"
        android:padding="12dp"
        android:layout_marginBottom="40dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>

```

11. activity_ticket_selection.xml

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F5F5F5"
    tools:context=".TicketSelectionActivity">

    <!-- Judul Halaman -->
    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/ticket_selection_title"
        android:textSize="24sp"
        android:textStyle="bold"
        android:textColor="#333333"
        android:layout_marginTop="24dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_marginTop="16dp"
        app:layout_constraintTop_toBottomOf="@id/tvTitle"
        app:layout_constraintBottom_toBottomOf="parent">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="20dp">

            <!-- TIKET 1: REGULAR -->
            <androidx.cardview.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginBottom="20dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="4dp">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="vertical"
                    android:padding="16dp">

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/ticket_regular"
                        android:textStyle="bold"
                        android:textSize="20sp"
                        android:textColor="#4CAF50"/>

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Rp 70.000"
                        android:textSize="18sp"
                        android:layout_marginBottom="8dp"/>

                    <!-- Deskripsi Tiket (Manual karena terlalu spesifik/panjang, atau bisa buat string baru jika mau diterjemahkan juga) -->
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="• Akses tak terbatas 1 Hari dalam bentuk Wristband\n• Pamflet stempel Hoyo FEST (1 Buah)\n• Akses masuk mulai pukul 11.00 WIB"
                        android:textColor="#666666"/>

                    <Button
                        android:id="@+id/btnRegular"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/btn_choose"
                        android:backgroundTint="#4CAF50"
                        android:layout_marginTop="12dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>

            <!-- TIKET 2: REGULAR 4 DAYS -->
            <androidx.cardview.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginBottom="20dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="4dp">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="vertical"
                    android:padding="16dp">

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/ticket_regular_4days"
                        android:textStyle="bold"
                        android:textSize="20sp"
                        android:textColor="#2196F3"/>

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Rp 230.000"
                        android:textSize="18sp"
                        android:layout_marginBottom="8dp"/>

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="• Akses tak terbatas 4 Hari dalam bentuk 4 Wristband berbeda\n• Pamflet stempel Hoyo FEST (4 Buah)\n• Akses masuk mulai pukul 11.00 WIB"
                        android:textColor="#666666"/>

                    <Button
                        android:id="@+id/btnRegular4Hari"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/btn_choose"
                        android:backgroundTint="#2196F3"
                        android:layout_marginTop="12dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>

            <!-- TIKET 3: VIP -->
            <androidx.cardview.widget.CardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginBottom="20dp"
                app:cardCornerRadius="12dp"
                app:cardElevation="4dp">

                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:orientation="vertical"
                    android:padding="16dp"
                    android:background="#FFF8E1"> <!-- Background sedikit kuning -->

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="@string/ticket_vip"
                        android:textStyle="bold"
                        android:textSize="20sp"
                        android:textColor="#FF9800"/>

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="Rp 500.000"
                        android:textSize="18sp"
                        android:layout_marginBottom="8dp"/>

                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="• Akses tak terbatas 1 hari\n• Jam masuk jalur prioritas 10.00 AM\n• Proritas masuk ke Zona Pengalaman Hoyoverse, semua booth game Hoyoverse, dan zona foto HoyoFEST\n• Dapatkan hadiah gratis di semua booth game Hoyoverse\n• Paspor stempel HoyoFEST 2025 eksklusif\n• Lanyard dan kartu identitas DIY eksklusif HoyoFEST 2025\n• Tiket undian"
                        android:textColor="#666666"/>

                    <Button
                        android:id="@+id/btnVvip"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/btn_choose"
                        android:backgroundTint="#FF9800"
                        android:layout_marginTop="12dp"/>
                </LinearLayout>
            </androidx.cardview.widget.CardView>

        </LinearLayout>
    </ScrollView>
</androidx.constraintlayout.widget.ConstraintLayout>

```
