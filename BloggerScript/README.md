# Panduan BloggerScript v1.2.0 Custom
## Sintaks
### BloggerScript
Mengambil sebagian postingan, hanya bisa mengambil maksimal 150 post.
```javascript
const bloggerFeed = new BloggerScript();

bloggerFeed.xhr('https://blog_url.blogspot.com/feeds/posts/default?alt=json-in-script&max-results=20', function(entry) {
  console.log(entry); //Output Array entry
});
```

yang sering digunakan

```html
<div class="class_name" data-label="Serie" data-max="6"></div>
```
```javascript
let custom_post = new BloggerScript(
  noImage: "URL_No_Thumbnail",
  sizeImage: 's320'
);

const get_custom_post = (e) => {
  let get_location = document.querySelector('.class_name');
  let entry = custom_post.getFeed(e);
  let html = '';
  entry.forEach(item => {
    html += ``;
  });
  if (get_location) {
    if (entry.length === 0) {
      get_location.innerHTML = `<span>No Post...</span>`;
    } else {
      get_location.innerHTML = html;
    }
  }
};

const run_custom_post = () => {
  let get_location = document.querySelector('.class_name');
  if (get_location) {
    let label_name = get_location.dataset.label || false;
    let max = get_location.dataset.max || false;
    custom_post.xhr(`/feeds/posts/default${label_name == false ? '' : `/-/${label_name}`}?alt=json-in-script&max-results=${max == false ? '' : `${max}`}`, get_custom_post);
  }
};

document.addEventListener('DOMContentLoaded', () => {
  run_custom_post();
});
```

### BloggerSitemap
Mengambil seluruh postingan yang ada, bisa mengambil lebih dari 150.
```javascript
const bloggerSitemap = new BloggerSitemap();

bloggerSitemap.run('https://blog_url.blogspot.com/feeds/posts/default', function(entry) {
  console.log(entry); //Output Array Entry
});
```

### BloggerRandom
```javascript
//Mengambil postingan random, hanya bisa mengambil maksimal 150 post.
const bloggerRandom = new BloggerRandom({
//Atur Jumlah Postingan yang ingin di tampilkan maksimal 150
  'jumlah': 10
});

bloggerRandom.run('https://blog_url.blogspot.com/feeds/posts/default', function(entry) {
  console.log(entry); //Output Array Entry
});
```

### BloggerRelated
```javascript
//Mengambil postingan per category, cocok untuk related post.
const bloggerRelated = new BloggerRelated({
  'labels': ['Blogger', 'Otomotif', 'Hiburan'],
  //Jumlah Post yang akan di ambil, maksimal post yang dapat di ambil adalah jumlah label di kalikan 15, contoh: 15x3 = 45.
  'jumlah': 10
});

bloggerRelated.run('https://blog_url.blogspot.com/feeds/posts/default', function(entry) {
  console.log(entry); //Output Array Entry
});
```

## Material
### No Thumbnail
+ `Horizontal` : https://1.bp.blogspot.com/-FReCec7n6RY/YSQPKYQ3GZI/AAAAAAAAALQ/68ope4pW8k0f4nEtHT74JUVroyigkNQtACLcBGAsYHQ/s320/No%2BImage%2BHorizontal.jpg
+ `Vertikal` : https://1.bp.blogspot.com/-XSp30PahyTM/YK37Rq_-M7I/AAAAAAAABCc/01K0sUhw-2YI7vr48wqMIAVoMLDEUdK2gCLcBGAsYHQ/s320/No%2BImage%2BBerkas%2BKita.jpg

### Filter Label
+ `Regex`
  
  + Score :
    ```javascript
    let filter_label = item.label.find(i => /^\d+(\.\d{1,2})$/.test(i)) || '0.0';
    ```
  + Tahun :

    ```javascript
    let filter_label = item.label.find(i => /^[0-9]{4}$/.test(i)) || '';
    ```

  + Musim :

    ```javascript
    let filter_label = item.label.find(i => /^([Ww]inter|[Ss]pring|[Ss]ummer|[Ff]all) [0-9]{4}$/.test(i)) || '';
    ```

  + Menit :

    ```javascript
    let filter_label = item.label.find(i => /^[0-9]{2} [Mm]in$/.test(i)) || '';
    ```

+ `Single Label`
  
  ```javascript
  let filter_label = item.label.find(i => ['Completed','Delay','Ongoing'].some(s => s == i));
  let filter_label = item.label.find(i => ['Completed','Delay','Ongoing'].includes(i));
  ```
+ `2 Label`
  
  ```javascript
  let label = item.label.filter(i => ['Action','Adventure','Drama','Fantasy','Horror','Romance','Sci-Fi'].some(s => s == i)).slice(0, 2);
  let filter_label = label.map(label => `<a href="/search/label/${label}?&max-results=7" rel="tag">${label}</a>`).join('');
  ```
+ `Multi Label`
  
  ```javascript
  let label = item.label.filter(i => ['Action','Adventure','Drama','Fantasy','Horror','Romance','Sci-Fi'].some(s => s == i));
  let filter_label = label.map(label => `<a href="/search/label/${label}?&max-results=7" rel="tag">${label}</a>`).join('');

  let label = item.label.filter(l => ['Action','Adventure','Drama','Fantasy','Horror','Romance','Sci-Fi'].includes(l));
  let filter_label = label.map(l => `<span>${l}</span>`).join('');
  ```

  ```javascript
  if (item.label.some(i => ['Action','Adventure','Drama','Fantasy','Horror','Romance','Sci-Fi'].includes(i))) {
    let filter_label = item.label.filter(i => ['Action','Adventure','Drama','Fantasy','Horror','Romance','Sci-Fi'].includes(i));
    filter_label.forEach((label, indexz) => {
      html += `<a href="/search/label/${label}?&max-results=7">${label}</a>`;
    })
  }
  ```

### Sinopsis/Summary

```javascript
if ('content' in item) {
  let newDiv = document.createElement('div');
  newDiv.innerHTML = item.content;

  // getElement
  let elSynopsis = newDiv.querySelector('ID/Class');

  // getText
  content = elSynopsis ? elSynopsis.innerText.slice(0, 200) : 'No Synopsis';
}
```
```javascript
let newDiv = document.createElement('div');
newDiv.innerHTML = 'content' in item ? item.content : 'Tidak ada Content';
let content = newDiv.innerText.slice(0, 150);
```

### Filter Array
```javascript
let entry = cp_chapter.getFeed(e);
// Menambil semua array kecuali label Series
let arr_ = entry.filter(item => !item.label.includes('Series'));
// Menambil semua array dengan label Chapter
let arr_Chapter = arr_.filter(item => item.label.includes('Chapter'));
```

## Fungsi warisan
### Fungsi Resize Image
```javascript
const bloggerFeed = new BloggerScript();

let image = 'image_url';
let newSizeImage = 's800-c-rw';
let newImage = bloggerFeed.resizeImage(image, newSizeImage);

console.log(newImage) // output image resized;
```

Adapun Format untuk ukuran gambar nya sebagai berikut:

1. s800-c => Konversi berdasarkan lebar gambar yakni 800.
2. w480-h360-c => Konversi berdasarkan gambar lebar 480 dan tinggi gambar 360.

Dukungan Format Webp;
untuk mengubah format gambar menjadi webp tambahkan "-rw" di akhir ukuran gambar.
1. s800-c-rw
2. w480-h360-c-rw
