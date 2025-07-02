# Gunakan Go Alpine sebagai base image. Alpine lebih kecil.
FROM golang:1.22-alpine AS builder

# Set working directory di dalam container
WORKDIR /app

# Salin go.mod dan go.sum terlebih dahulu untuk caching dependensi yang efisien
COPY go.mod .
COPY go.sum .

# Download Go modules/dependensi
# Ini akan mengambil semua dependensi yang dibutuhkan oleh proyek Anda
RUN go mod download

# Salin sisa kode aplikasi Anda ke dalam container
COPY . .

# Bangun aplikasi Go Anda
# -o main: nama file executable yang dihasilkan akan 'main'
# -ldflags "-s -w": flags linker untuk mengurangi ukuran binary (menghilangkan symbol table dan debug info)
# ./main.go: jalur ke file main.go Anda relatif terhadap WORKDIR (/app)
RUN go build -ldflags "-s -w" -o main ./main.go

# --- Stage 2: Final Image (Distroless/Scratch untuk ukuran terkecil) ---
# Gunakan image scratch yang sangat minimal. Ini tidak memiliki OS atau shell, hanya binary Anda.
FROM alpine:3.21

# Salin binary yang sudah di-build dari stage 'builder'
# Binary 'main' yang sudah dioptimalkan akan disalin ke root folder image final
COPY --from=builder /app/main .

COPY .env /
# Expose the application port
EXPOSE 8080 

# Command to run the application
CMD ["./main"]  