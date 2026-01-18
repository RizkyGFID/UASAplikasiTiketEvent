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
