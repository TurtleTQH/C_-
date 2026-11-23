#include <stdio.h>
#include <string.h>

#define MAX 200

typedef struct {
    char bienSo[15];
    int loaiXe;         /* 1: xe dap, 2: xe may, 3: o to */
    int gioVao, phutVao;
    int dangGui;        /* 1: dang trong bai, 0: da ra */
} Xe;

/* ---------- Bien toan cuc ---------- */
Xe ds[MAX];             /* danh sach xe */
int n = 0;              /* so xe da luu */
long doanhThu[4] = {0, 0, 0, 0};   /* doanh thu: chi so 1, 2, 3 */

/* ---------- Ham ho tro ---------- */

/* Doi gio phut sang so phut */
int doiRaPhut(int gio, int phut) {
    return gio * 60 + phut;
}

/* Lam tron so phut len thanh so gio */
int lamTronGioLen(int soPhut) {
    int gio = soPhut / 60;
    if (soPhut % 60 > 0) {
        gio = gio + 1;
    }
    return gio;
}

/* Kiem tra gio phut hop le */
int gioPhutHopLe(int gio, int phut) {
    if (gio < 0 || gio > 23) return 0;
    if (phut < 0 || phut > 59) return 0;
    return 1;
}

/* Suc chua tung loai xe */
int sucChua(int loai) {
    switch (loai) {
        case 1: return 20;
        case 2: return 50;
        case 3: return 10;
    }
    return 0;
}

/* Ten loai xe */
const char *tenLoai(int loai) {
    switch (loai) {
        case 1: return "Xe dap";
        case 2: return "Xe may";
        case 3: return "O to";
    }
    return "?";
}

/* Dem so xe dang gui theo loai (vong lap) */
int demXe(int loai) {
    int i, dem = 0;
    for (i = 0; i < n; i++) {
        if (ds[i].dangGui == 1 && ds[i].loaiXe == loai) {
            dem++;
        }
    }
    return dem;
}

/* Tim xe dang gui theo bien so, tra ve con tro (NULL neu khong co) */
Xe* timXe(Xe ds[], int n, const char *bienSo) {
    int i;
    for (i = 0; i < n; i++) {
        if (ds[i].dangGui == 1 && strcmp(ds[i].bienSo, bienSo) == 0) {
            return &ds[i];
        }
    }
    return NULL;
}

/* Tinh tien gui xe */
long tinhTien(const Xe *x, int gioRa, int phutRa) {
    int soPhut = doiRaPhut(gioRa, phutRa) - doiRaPhut(x->gioVao, x->phutVao);
    long tien = 0;
    int soGio;

    switch (x->loaiXe) {
        case 1:                         /* xe dap */
            tien = 2000;
            break;
        case 2:                         /* xe may */
            tien = 4000;
            if (soPhut > 8 * 60) {
                tien = tien * 2;
            }
            break;
        case 3:                         /* o to */
            soGio = lamTronGioLen(soPhut);
            if (soGio < 1) {
                soGio = 1;
            }
            tien = soGio * 25000L;
            break;
    }
    return tien;
}

/* ---------- Cac chuc nang ---------- */

/* 1. Xe vao bai */
void xeVaoBai() {
    Xe xe;

    printf("Loai xe (1-Xe dap, 2-Xe may, 3-O to): ");
    scanf("%d", &xe.loaiXe);
    if (xe.loaiXe < 1 || xe.loaiXe > 3) {
        printf("Loai xe khong hop le!\n");
        return;
    }
    if (demXe(xe.loaiXe) >= sucChua(xe.loaiXe)) {
        printf("Bai da het cho cho loai xe nay!\n");
        return;
    }

    printf("Nhap bien so: ");
    scanf("%14s", xe.bienSo);
    if (timXe(ds, n, xe.bienSo) != NULL) {
        printf("Xe nay dang gui trong bai!\n");
        return;
    }

    printf("Nhap gio va phut vao: ");
    scanf("%d %d", &xe.gioVao, &xe.phutVao);
    if (gioPhutHopLe(xe.gioVao, xe.phutVao) == 0) {
        printf("Gio phut khong hop le!\n");
        return;
    }

    xe.dangGui = 1;
    ds[n] = xe;
    n++;
    printf("Da nhan xe vao bai.\n");
}

/* 2. Xe ra bai */
void xeRaBai() {
    char bienSo[15];
    int gioRa, phutRa, soPhut;
    Xe *x;

    printf("Nhap bien so xe ra: ");
    scanf("%14s", bienSo);
    x = timXe(ds, n, bienSo);
    if (x == NULL) {
        printf("Khong tim thay xe trong bai!\n");
        return;
    }

    printf("Nhap gio va phut ra: ");
    scanf("%d %d", &gioRa, &phutRa);
    if (gioPhutHopLe(gioRa, phutRa) == 0) {
        printf("Gio phut khong hop le!\n");
        return;
    }
    soPhut = doiRaPhut(gioRa, phutRa) - doiRaPhut(x->gioVao, x->phutVao);
    if (soPhut < 0) {
        printf("Gio ra phai sau gio vao!\n");
        return;
    }

    printf("Thoi gian gui: %d gio %d phut\n", soPhut / 60, soPhut % 60);
    printf("Tien phai tra: %ld dong\n", tinhTien(x, gioRa, phutRa));

    doanhThu[x->loaiXe] = doanhThu[x->loaiXe] + tinhTien(x, gioRa, phutRa);
    x->dangGui = 0;
}

/* 3. Hien thi danh sach xe dang gui va cho trong */
void hienThiDanhSach() {
    int i, loai;

    printf("Danh sach xe dang gui:\n");
    for (i = 0; i < n; i++) {
        if (ds[i].dangGui == 1) {
            printf("  %-14s %-8s vao luc %02d:%02d\n", ds[i].bienSo,
                   tenLoai(ds[i].loaiXe), ds[i].gioVao, ds[i].phutVao);
        }
    }
    printf("So cho trong:\n");
    for (loai = 1; loai <= 3; loai++) {
        printf("  %-8s: %d cho\n", tenLoai(loai), sucChua(loai) - demXe(loai));
    }
}

/* 4. Tim xe theo bien so hoac mot phan bien so */
void timTheoBienSo() {
    char tuKhoa[15];
    int i, thay = 0;

    printf("Nhap bien so (hoac mot phan): ");
    scanf("%14s", tuKhoa);
    for (i = 0; i < n; i++) {
        if (ds[i].dangGui == 1 && strstr(ds[i].bienSo, tuKhoa) != NULL) {
            printf("  %-14s %-8s vao luc %02d:%02d\n", ds[i].bienSo,
                   tenLoai(ds[i].loaiXe), ds[i].gioVao, ds[i].phutVao);
            thay = 1;
        }
    }
    if (thay == 0) {
        printf("Khong tim thay xe nao.\n");
    }
}

/* 5. Thong ke doanh thu */
void thongKeDoanhThu() {
    int loai;
    long tong = 0;

    for (loai = 1; loai <= 3; loai++) {
        printf("  %-8s: %ld dong\n", tenLoai(loai), doanhThu[loai]);
        tong = tong + doanhThu[loai];
    }
    printf("  Tong cong: %ld dong\n", tong);
}

/* 6. Liet ke xe gui qua 8 gio */
void xeGuiQuaLau() {
    int gio, phut, i, soPhut, thay = 0;

    printf("Nhap gio va phut hien tai: ");
    scanf("%d %d", &gio, &phut);
    if (gioPhutHopLe(gio, phut) == 0) {
        printf("Gio phut khong hop le!\n");
        return;
    }
    for (i = 0; i < n; i++) {
        if (ds[i].dangGui == 1) {
            soPhut = doiRaPhut(gio, phut) - doiRaPhut(ds[i].gioVao, ds[i].phutVao);
            if (soPhut > 8 * 60) {
                printf("  %-14s %-8s da gui %d gio %d phut\n", ds[i].bienSo,
                       tenLoai(ds[i].loaiXe), soPhut / 60, soPhut % 60);
                thay = 1;
            }
        }
    }
    if (thay == 0) {
        printf("Khong co xe nao gui qua 8 gio.\n");
    }
}

/* ---------- Chuong trinh chinh ---------- */
int main() {
    int chon;

    do {
        printf("\n===== QUAN LY BAI GUI XE =====\n");
        printf("1. Xe vao bai\n");
        printf("2. Xe ra bai\n");
        printf("3. Danh sach xe dang gui, cho trong\n");
        printf("4. Tim xe theo bien so\n");
        printf("5. Thong ke doanh thu\n");
        printf("6. Xe gui qua 8 gio\n");
        printf("0. Thoat\n");
        printf("Chon: ");
        scanf("%d", &chon);

        switch (chon) {
            case 1: xeVaoBai(); break;
            case 2: xeRaBai(); break;
            case 3: hienThiDanhSach(); break;
            case 4: timTheoBienSo(); break;
            case 5: thongKeDoanhThu(); break;
            case 6: xeGuiQuaLau(); break;
            case 0: printf("Tam biet!\n"); break;
            default: printf("Chuc nang khong hop le!\n");
        }
    } while (chon != 0);

    return 0;
}

