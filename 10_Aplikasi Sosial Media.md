<h2 align="center"><strong>APLIKASI SOSIAL MEDIA</strong></h2>
</p>

<br>

<p align="center">
  <strong> ANGGOTA KELOMPOK:</strong><br>
  Zahra Tsuroyya Poetri (2311102127)<br>
  Lutfiana Isnaeni Lathifah (2311102165)<br>
  Ria Wulandari (2311102173)<br>
  Fatik Nurimamah (2311102190)<br>
  S1 IF 11 05

### Source Code Program: 
```go
package main

import (
	"fmt"
	"time"
)

// Tipe bentukan untuk menyimpan informasi pengguna
type User struct {
	Username    string      // Nama pengguna
	Password    string      // Kata sandi pengguna
	Profile     string      // Informasi profil pengguna
	Friends     [100]string // Array statis untuk menyimpan daftar teman
	FriendAddedTime [100]int64  // Array statis untuk menyimpan waktu penambahan teman
	FriendCount int         // Jumlah teman yang ada
	Status      [100]string // Array statis untuk menyimpan daftar status
	StatusCount int         // Jumlah status yang ada
}

// Variabel global untuk menyimpan data utama
var users [100]User
var userCount int       // Jumlah pengguna yang terdaftar
var currentUser *User   // Pointer ke pengguna yang sedang login

func main() {
	for {
		fmt.Println("\n=== Selamat Datang ===")
		fmt.Println("1. Registrasi")
		fmt.Println("2. Masuk")
		fmt.Println("3. Keluar")
		var choice int
		fmt.Print("Pilih menu: ")
		fmt.Scan(&choice)

		if choice == 1 {
			register()
		} else if choice == 2 {
			if login() {
				home()
			}
		} else if choice == 3 {
			fmt.Println("Keluar dari aplikasi.")
			return
		} else {
			fmt.Println("Pilihan tidak valid.")
		}
	}
}

// Registrasi pengguna baru
func register() {
	if userCount >= 100 {
		fmt.Println("Maksimum pengguna tercapai.")
		return
	}

	var username, password string
	fmt.Print("\nMasukkan username: ")
	fmt.Scan(&username)
	fmt.Print("Masukkan password: ")
	fmt.Scan(&password)

	if sequentialSearch(username) != -1 {
		fmt.Println("Username sudah digunakan.")
		return
	}

	users[userCount] = User{
		Username: username,
		Password: password,
	}
	userCount++
	fmt.Println("Registrasi berhasil.")
}

// Login pengguna
func login() bool {
	var username, password string
	fmt.Print("\nMasukkan username: ")
	fmt.Scan(&username)
	fmt.Print("Masukkan password: ")
	fmt.Scan(&password)

	index := sequentialSearch(username)
	if index != -1 && users[index].Password == password {
		currentUser = &users[index]
		fmt.Println("Login berhasil.")
		return true
	}

	fmt.Println("Username atau password salah.")
	return false
}

// Sequential search untuk mencari pengguna berdasarkan username
func sequentialSearch(username string) int {
	for i := 0; i < userCount; i++ {
		if users[i].Username == username {
			return i
		}
	}
	return -1
}

// Home menu setelah login
func home() {
	for {
		fmt.Println("\n=== Menu ===")
		fmt.Println("1. Lihat Status Semua Pengguna")
		fmt.Println("2. Tambah Status")
		fmt.Println("3. Kelola Teman")
		fmt.Println("4. Edit Profil")
		fmt.Println("5. Cari Pengguna")
		fmt.Println("6. Keluar")
		var choice int
		fmt.Print("Pilih menu: ")
		fmt.Scan(&choice)

		if choice == 1 {
			viewAllStatus()
		} else if choice == 2 {
			addStatus()
		} else if choice == 3 {
			manageFriends()
		} else if choice == 4 {
			editProfile()
		} else if choice == 5 {
			searchUsers()
		} else if choice == 6 {
			fmt.Println("Keluar berhasil.")
			currentUser = nil
			return
		} else {
			fmt.Println("Pilihan tidak valid.")
		}
	}
}

// Menampilkan semua status pengguna
func viewAllStatus() {
	for i := 0; i < userCount; i++ {
		fmt.Printf("\nStatus dari %s:\n", users[i].Username)
		for j := 0; j < users[i].StatusCount; j++ {
			fmt.Printf("%d. %s\n", j+1, users[i].Status[j])
		}
	}

	fmt.Println("\nIngin memberikan komentar pada status?")
	fmt.Println("1. Ya")
	fmt.Println("2. Tidak")
	var choice int
	fmt.Print("Pilih: ")
	fmt.Scan(&choice)

	if choice == 1 {
		addComment()
	}
}

// Menambahkan komentar pada status
func addComment() {
	var username string
	fmt.Print("\nMasukkan username pemilik status: ")
	fmt.Scan(&username)

	userIndex := sequentialSearch(username)
	if userIndex == -1 {
		fmt.Println("Pengguna tidak ditemukan.")
		return
	}

	var statusIndex int
	fmt.Print("Masukkan nomor status yang ingin dikomentari: ")
	fmt.Scan(&statusIndex)

	if statusIndex < 1 || statusIndex > users[userIndex].StatusCount {
		fmt.Println("Nomor status tidak valid.")
		return
	}

	var comment string
	fmt.Print("Masukkan komentar: ")
	fmt.Scan(&comment)

	fmt.Printf("Komentar Anda pada status '%s': %s\n", users[userIndex].Status[statusIndex-1], comment)
	fmt.Println("Komentar berhasil ditambahkan.")
}


// Menambahkan status baru
func addStatus() {
	if currentUser.StatusCount >= 100 {
		fmt.Println("Maksimum status tercapai.")
		return
	}

	var status string
	fmt.Print("Masukkan status baru: ")
	fmt.Scan(&status)
	currentUser.Status[currentUser.StatusCount] = status
	currentUser.StatusCount++
	fmt.Println("Status berhasil ditambahkan.")
}

// Mengelola teman
func manageFriends() {
	for {
		fmt.Println("\n=== Kelola Teman ===")
		fmt.Println("1. Tambah Teman")
		fmt.Println("2. Hapus Teman")
		fmt.Println("3. Lihat Teman")
		fmt.Println("4. Kembali")
		var choice int
		fmt.Print("Pilih menu: ")
		fmt.Scan(&choice)

		if choice == 1 {
			addFriend()
		} else if choice == 2 {
			removeFriend()
		} else if choice == 3 {
			viewFriends()
		} else if choice == 4 {
			return
		} else {
			fmt.Println("Pilihan tidak valid.")
		}
	}
}

// Menambahkan teman baru
func addFriend() {
	if currentUser.FriendCount >= 100 {
		fmt.Println("Maksimum teman tercapai.")
		return
	}

	var friend string
	fmt.Print("Masukkan username teman: ")
	fmt.Scan(&friend)

	// Cek apakah pengguna sudah terdaftar
	for i := 0; i < userCount; i++ {
		if users[i].Username == friend {
			currentUser.Friends[currentUser.FriendCount] = friend
			currentUser.FriendAddedTime[currentUser.FriendCount] = time.Now().Unix() // Simpan waktu penambahan
			currentUser.FriendCount++
			fmt.Println("Teman berhasil ditambahkan.")
			return
		}
	}

	// Jika pengguna tidak ditemukan, tambahkan sebagai pengguna baru
	if userCount < 100 {
		users[userCount] = User{
			Username: friend,
		}
		userCount++
		currentUser.Friends[currentUser.FriendCount] = friend
		currentUser.FriendAddedTime[currentUser.FriendCount] = time.Now().Unix() // Simpan waktu penambahan
		currentUser.FriendCount++
		fmt.Println("Teman baru berhasil didaftarkan dan ditambahkan.")
	} else {
		fmt.Println("Maksimum pengguna tercapai. Tidak dapat menambahkan teman baru.")
	}
}

// Menghapus teman dari daftar
func removeFriend() {
	var friend string
	fmt.Print("Masukkan username teman yang ingin dihapus: ")
	fmt.Scan(&friend)

	for i := 0; i < currentUser.FriendCount; i++ {
		if currentUser.Friends[i] == friend {
			for j := i; j < currentUser.FriendCount-1; j++ {
				currentUser.Friends[j] = currentUser.Friends[j+1]
			}
			currentUser.FriendCount--
			fmt.Println("Teman berhasil dihapus.")
			return
		}
	}

	fmt.Println("Teman tidak ditemukan.")
}

// Melihat daftar teman dan memilih urutan
func viewFriends() {
	if currentUser.FriendCount == 0 {
		fmt.Println("Tidak ada teman.")
		return
	}

	// Menampilkan pilihan pengurutan
	fmt.Println("\nUrutkan teman berdasarkan:")
	fmt.Println("1. Username")
	fmt.Println("2. Waktu Penambahan")
	var order int
	fmt.Print("Pilih urutan: ")
	fmt.Scan(&order)

	// Melakukan pengurutan sesuai pilihan
	if order == 1 {
		sortFriendsByUsername()
	} else if order == 2 {
		sortFriendsByTimeAdded()
	} else {
		fmt.Println("Pilihan tidak valid.")
		return
	}

	// Menampilkan daftar teman setelah diurutkan
	displayFriends()
}

// Menampilkan daftar teman
func displayFriends() {
	fmt.Println("\nDaftar Teman Anda:")
	for i := 0; i < currentUser.FriendCount; i++ {
		fmt.Println(currentUser.Friends[i])
	}
}

// Mengurutkan teman berdasarkan username menggunakan Selection Sort
func sortFriendsByUsername() {
	for i := 0; i < currentUser.FriendCount-1; i++ {
		minIndex := i
		for j := i + 1; j < currentUser.FriendCount; j++ {
			if currentUser.Friends[j] < currentUser.Friends[minIndex] {
				minIndex = j
			}
		}
		// Tukar elemen
		currentUser.Friends[i], currentUser.Friends[minIndex] = currentUser.Friends[minIndex], currentUser.Friends[i]
	}
	fmt.Println("Teman berhasil diurutkan berdasarkan username.")
}

// Mengurutkan teman berdasarkan waktu penambahan (dari terbaru ke terlama) menggunakan Insertion Sort
func sortFriendsByTimeAdded() {
	for i := 1; i < currentUser.FriendCount; i++ {
		keyFriend := currentUser.Friends[i]
		keyTime := currentUser.FriendAddedTime[i]
		j := i - 1

		// Geser elemen-elemen yang lebih lama ke depan
		for j >= 0 && currentUser.FriendAddedTime[j] < keyTime {
			currentUser.Friends[j+1] = currentUser.Friends[j]
			currentUser.FriendAddedTime[j+1] = currentUser.FriendAddedTime[j]
			j = j - 1
		}
		currentUser.Friends[j+1] = keyFriend
		currentUser.FriendAddedTime[j+1] = keyTime
	}
	fmt.Println("Teman berhasil diurutkan berdasarkan waktu penambahan.")
}

// Mengedit profil pengguna
func editProfile() {
	var newProfile string
	fmt.Print("Masukkan informasi profil baru: ")
	fmt.Scan(&newProfile)
	currentUser.Profile = newProfile
	fmt.Println("Profil berhasil diperbarui.")
}

// Mencari pengguna berdasarkan username
func searchUsers() {
	var search string
	fmt.Print("Masukkan username yang dicari: ")
	fmt.Scan(&search)

	index := binarySearch(search)
	if index != -1 {
		fmt.Printf("\nUsername: %s\n", users[index].Username)
		fmt.Printf("Profil: %s\n", users[index].Profile)
	} else {
		fmt.Println("Pengguna tidak ditemukan.")
	}
}

// Binary search untuk mencari pengguna (dengan asumsi data sudah terurut)
func binarySearch(username string) int {
	low, high := 0, userCount-1
	for low <= high {
		mid := (low + high) / 2
		if users[mid].Username == username {
			return mid
		} else if users[mid].Username < username {
			low = mid + 1
		} else {
			high = mid - 1
		}
	}
	return -1
}
```
