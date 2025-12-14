# RasganeJS
Pustaka JavaScript (Library) ringan untuk mempermudah manipulasi elemen, feed, dan fitur interaktif pada template Blogger.

## Pemasangan
Sebelum menggunakan fitur-fiturnya, Anda harus memasang kode RasganeJS ke dalam template Blogger Anda.

### Cara Pasang:
+ Masuk ke Blogger > Tema > Edit HTML.
+ Cari kode `</body>` (biasanya ada di paling bawah).
+ Tempelkan kode berikut tepat di atas `</body>`.

```javascript
//===================================
//===================================
// RasganeJS v1.1.1
// Update : 12/12/2025
//===================================
//===================================
let setting_rasganejs={max:100};
class FeedScript{constructor(e={}){this._config=e}err(e){return console.log(e),e}createRT(t){let r="",s="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789",n=s.length;for(let e=0;e<t;e++)r+=s.charAt(Math.floor(Math.random()*n));return r}resizeImage(e,t=!1){var r,s;return t?(r=/\/(s|w|h)\d{1,4}-((w|s|h)(\d{1,4})+-)?(c{1,2}|p-k-no-nu|rw)/gi,s=/\=(s|w|h)\d{1,4}-((w|s|h)(\d{1,4})+-)?(c{1,2}|p-k-no-nu|rw)/gi,(e=/\-(rw)$/.test(t)?e.replace(/\.(gif|jpe?g|tiff?|png|bmp)$/,".webp"):e).match(r)?e.replace(r,"/"+t):e.match(s)?e.replace(s,"="+t):e):e}shuffle(e){var t,r,s=e.length;if(0===s)return!1;for(;--s;)t=Math.floor(Math.random()*(s+1)),r=e[s],e[s]=e[t],e[t]=r;return e}shuffle2(e,t){return Math.floor(Math.random()*(t-e))+e}sort(e,t){if("Update"==t||"Added"==t){let r="Update"==t?"updated":"published";e=e.sort(function(e,t){return e[r]<t[r]?-1:e[r]>t[r]?1:0}).reverse()}else"A-Z"!=t&&"Z-A"!=t||(e=e.sort((e,t)=>e.title.localeCompare(t.title,void 0,{numeric:!0})),"Z-A"==t&&(e=e.reverse()));return e}xhr(e,t=this.err){var r=window.XMLHttpRequest?new XMLHttpRequest:new ActiveXObject("Microsoft.XMLHTTP");r.onreadystatechange=function(){var e;4==this.readyState&&200==this.status||304==this.status?(e=this.responseText,e=JSON.parse(e.substring(e.indexOf("{"),e.lastIndexOf("}")+1)),t&&t(e)):4==this.readyState&&t&&t({})},r.open("GET",e,!0),r.send()}xhr2(e,t=this.err){var r=document.createElement("script"),s="xhr2"+this.createRT(7);return window[s]=function(e){return t(e)},r.src=e+"&callback=window."+s,r.onerror=function(e){console.log(e),t({})},r.async=!0,(document.body||document.getElementsByTagName("body")[0]).appendChild(r)}getImage(e){var t,r,s,n;return"media$thumbnail"in e?n=this.resizeImage(e.media$thumbnail.url,this._config.sizeImage):"content"in e&&(t=(e=e.content.$t).indexOf("<img"),r=e.indexOf('src="',t),s=e.indexOf('"',r+5),n=e.substr(r+5,s-r-5),-1!=t)&&-1!=r&&-1!=s&&""!=n||(n=this._config.noImage||""),/^https?:\/\/img\.youtube\.com\/vi\/.+\/default\.jpg$/.test(n)?n.replace("default.jpg","maxresdefault.jpg"):n}getTime(e){var t,r,s;return!!/([12]\d{3}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01]))/.test(e)&&(t=e.substring(0,4),r=e.substring(5,7),e=e.substring(8,10),(s=new Array)[1]="Jan",s[2]="Feb",s[3]="Mar",s[4]="Apr",s[5]="May",s[6]="Jun",s[7]="Jul",s[8]="Aug",s[9]="Sep",s[10]="Oct",s[11]="Nov",s[12]="Dec",e+" "+s[parseInt(r,10)]+" "+t)}getFeed(t){var r=new Array;if(t.feed&&t.feed.entry)for(let e=0;e<t.feed.entry.length;e++){var s=t.feed.entry[e],n={};n.id=s.id.$t.split(".post-")[1],n.title=s.title.$t,n.link=s.link.find(e=>"alternate"==e.rel).href,n.enclosure=s.link.filter(e=>"enclosure"==e.rel).map(e=>({href:e.href,type:e.type})),n.image=this.getImage(s),n.label=s.category?.map(e=>e.term)||[],n.date=this.getTime(s.published.$t),n.comment=s.thr$total?.$t||0,n.published=s.published.$t,n.updated=s.updated.$t,"summary"in s&&(n.summary=s.summary.$t),"content"in s&&(n.content=s.content.$t),"author"in s&&(n.author={name:s.author[0]?.name?.$t||"Anonymous",url:s.author[0]?.uri?.$t||"",image:s.author[0]?.gd$image?.src||""}),r.push(n)}return r}}class FeedRandom extends FeedScript{constructor(e={}){super(e),this._config.jumlah=e.jumlah||10}run(e,t=this.err){const r=this;this.xhr(e+("?alt=json-in-script&max-results="+setting_rasganejs.max),function(e){0<(e=r.getFeed(e)).length?(e=e.sort(()=>.5-Math.random()).slice(0,r._config.jumlah),t(e)):t([])})}}class FeedRelated extends FeedScript{constructor(e={}){super(e)}run(t,r=this.err,e=!0){let s=this,n=0,a=document.location.pathname,o=s._config.labels,i=s._config.jumlah,d=e?"xhr":"xhr2";if(s._config.arr=new Array,"undefined"==o||""==o||0==o.length)return r([]);o.forEach(e=>{s[d](t+`/-/${e}?alt=json-in-script&max-results=`+i,e=>{if(s.getFeed(e).forEach(t=>{s._config.arr.some(e=>e.id==t.id)||s._config.arr.push(t)}),++n==o.length)return 0==s._config.arr.length?r([]):(e=s._config.arr.map(e=>new URL(e.link).pathname==a).indexOf(!0),s._config.arr.splice(e,1),e=s.shuffle(s._config.arr).slice(0,i),r(e))})})}}class FeedSitemap extends FeedScript{constructor(e){super(e),this._settings={"start-index":1,"max-results":110,"total-get":0,posts:new Array}}alphaSort(e){let r=new Array,t=new Array,s="";0!=e.length&&this.sort(e,"A-Z").forEach(e=>{var t=e.title.charAt(0).toLowerCase();-1==s.indexOf(t)?(s+=t,r[t]=[e]):r[t].push(e)});for(const e in r)if(Object.hasOwnProperty.call(r,e)){const s=r[e];t.push({id:e,items:s})}return t}run(r,s=this.err,n=!0){const a=this._settings,o=this._config,e=o.order||"updated",t=`${r}?alt=json-in-script&start-index=${a["start-index"]}&max-results=${a["max-results"]}&orderby=`+e,i=n?"xhr":"xhr2";a["total-get"]++,this[i](t,e=>{var t;"entry"in e.feed?(t=e.feed.openSearch$totalResults.$t||0,Array.prototype.push.apply(a.posts,this.getFeed(e)),e.feed.entry.length>=a["max-results"]?(a["start-index"]+=a["max-results"],o.firstContent&&1==a["total-get"]&&s(a.posts,a["total-get"],!1,t),this.run(r,s,n)):s(a.posts,a["total-get"],!0,t)):s(a.posts,a["total-get"],!0,a.posts.length)})}}class FeedSitemap2 extends FeedScript{constructor(e={}){super(e),this._config.maxResults=e.maxResults||setting_rasganejs.max||100,this._settings={"start-index":1,"total-get":0,posts:new Array,_estimatedTotal:0}}alphaSort(e){let r=new Array,t=new Array,s="";0!==e.length&&this.sort(e,"A-Z").forEach(e=>{var t=e.title.charAt(0).toLowerCase();-1===s.indexOf(t)?(s+=t,r[t]=[e]):r[t].push(e)});for(const e in r)if(Object.hasOwnProperty.call(r,e)){const s=r[e];t.push({id:e,items:s})}return t}run(r,s=this.err,n=!0){const a=this._settings,o=this._config;var e=o.order||"updated",e=`${r}?alt=json-in-script&start-index=${a["start-index"]}&max-results=${o.maxResults}&orderby=`+e,t=n?"xhr":"xhr2";a["total-get"]++,this[t](e,e=>{var t=(e.feed&&e.feed.entry?e.feed.entry:[]).length;a._estimatedTotal=parseInt(e.feed.openSearch$totalResults.$t,10),0<t?(Array.prototype.push.apply(a.posts,this.getFeed(e)),a["start-index"]=a.posts.length+1,o.firstContent&&(t=a._estimatedTotal>a.posts.length?a._estimatedTotal:a.posts.length,s(a.posts,a["total-get"],!1,t)),this.run(r,s,n)):s(a.posts,a["total-get"],!0,a.posts.length)})}}class FeedComments extends FeedScript{constructor(e={}){super(e),this._config.maxResults=e.maxResults||10}getCommentData(t){var r=[];if(t.feed&&t.feed.entry)for(let e=0;e<t.feed.entry.length;e++){var s=t.feed.entry[e],n={};n.id=s.id.$t.split(".post-")[1],n.link=s.link.find(e=>"alternate"==e.rel).href,n.content=s.content.$t,n.author={name:s.author[0].name.$t,uri:s.author[0].uri?s.author[0].uri.$t:"",email:s.author[0].email?s.author[0].email.$t:"",image:s.author[0].gd$image.src||""},n.published=s.published.$t,n.updated=s.updated.$t,r.push(n)}return r.slice(0,this._config.maxResults)}run(e,t=this.err,r=!1){var s="&max-results="+this._config.maxResults;r?this.xhr2(e+"?alt=json-in-script"+s,e=>{e=this.getCommentData(e),t(e)}):this.xhr(e+"?alt=json-in-script"+s,e=>{e=this.getCommentData(e),t(e)})}xhr(e,t=this.err){var r=window.XMLHttpRequest?new XMLHttpRequest:new ActiveXObject("Microsoft.XMLHTTP");r.onreadystatechange=function(){var e;4==this.readyState&&200==this.status||304==this.status?(e=this.responseText,e=JSON.parse(e.substring(e.indexOf("{"),e.lastIndexOf("}")+1)),t&&t(e)):4==this.readyState&&t&&t({})},r.open("GET",e,!0),r.send()}xhr2(e,t=this.err){let r=document.createElement("script"),s="xhr2"+this.createRT(7);window[s]=function(e){t(e),delete window[s]},r.src=e+"&callback=window."+s,r.onerror=function(){console.log("Error loading JSONP script."),t({}),delete window[s]},r.async=!0,(document.body||document.getElementsByTagName("body")[0]).appendChild(r)}}const get_moveElement=(e,t,r)=>{e=document.querySelector(e),(t=document.querySelector(t))&&(e?t.appendChild(e):t.innerHTML=r)},get_moveElement2=(e,t)=>{e=document.querySelector(e),(t=document.querySelector(t))&&(e?t.appendChild(e):t.remove())},bloggerFeed_random_123=new FeedScript,get_Random=(e,t)=>{const r=document.querySelector(e);r?(t=`/feeds/posts/default${t?"/-/"+t:""}?alt=json-in-script&max-results=`+setting_rasganejs.max,bloggerFeed_random_123.xhr(t,e=>{const t=(e=bloggerFeed_random_123.getFeed(e)).at(Math.floor(Math.random()*e.length)).link;r.addEventListener("click",()=>{window.location.href=t})})):console.error("Element not found for selector:",e)},get_up_button=(e,t)=>{const r=document.querySelector(e);r&&(r.addEventListener("click",()=>{window.scrollTo({top:0,behavior:"smooth"})}),t)&&window.addEventListener("scroll",()=>{100<window.scrollY?r.classList.add(t):r.classList.remove(t)})},get_year=e=>{(e=document.querySelector(e))&&(e.innerHTML=(new Date).getFullYear())},get_tabs=()=>{document.querySelectorAll("nav[data-tabs]").forEach(r=>{r.querySelectorAll(".tab_cp__").forEach(e=>{e.addEventListener("click",function(){var e=this.getAttribute("data-name"),t=r.getAttribute("data-tabs");r.querySelectorAll(".tab_cp__").forEach(e=>e.classList.remove("active")),document.querySelectorAll(t).forEach(e=>{e.classList.remove("active"),e.style.display="none"}),this.classList.add("active"),(t=document.querySelector(t+`[data-name="${e}"]`)).classList.add("active"),t.style.display="block"})})})},get_updateTimes=(d={})=>{const l={target:d.target||".time",ago:void 0===d.ago||d.ago,format:{now:"now",minute:"minute",hour:"hour",day:"day",month:"month",year:"year",second:"second",...d.format}};var e=document.querySelectorAll(l.target);0===e.length?console.warn(`get_updateTimes: Tidak ada elemen ditemukan untuk selector "${l.target}"`):e.forEach(e=>{var t=e.getAttribute("datetime")||e.textContent;t&&(e.textContent=(e=>{var e=new Date(e),t=new Date,r=Math.floor((t-e)/1e3);if(r<1)return l.format.now;for(const i of[{label:"year",seconds:31536e3},{label:"month",seconds:2592e3},{label:"day",seconds:86400},{label:"hour",seconds:3600},{label:"minute",seconds:60}]){var s=Math.floor(r/i.seconds);if(1<=s){const n=l.format[i.label],a=!d.format||!d.format[i.label],o=1!==s&&a?"s":"";return s+" "+n+o+(l.ago?" ago":"")}}const n=l.format.second,a=!d.format||!d.format.second,o=1!==r&&a?"s":"";return r+" "+n+o+(l.ago?" ago":"")})(t))})},get_klik1=(e,t,r,s)=>{r=document.querySelector(r);const n=document.querySelector(s);r&&n?r.addEventListener("click",()=>{"add"===e?t.startsWith(".")?n.classList.toggle(t.substring(1)):t.startsWith("#")&&(n.id===t.substring(1)?n.id="":n.id=t.substring(1)):"remove"===e&&(t.startsWith(".")?n.classList.remove(t.substring(1)):t.startsWith("#")&&n.id===t.substring(1)&&(n.id=""))}):console.error("Element not found. Make sure the class/id provided is correct.")},get_klik2=(t,r,e,s)=>{const n=document.querySelector(e),a=document.querySelector(s);n&&a?(n.addEventListener("click",e=>{e.stopPropagation(),"add"===t?r.startsWith(".")?a.classList.toggle(r.substring(1)):r.startsWith("#")&&(a.id===r.substring(1)?a.id="":a.id=r.substring(1)):"remove"===t&&(r.startsWith(".")?a.classList.remove(r.substring(1)):r.startsWith("#")&&a.id===r.substring(1)&&(a.id=""))}),document.addEventListener("click",e=>{n.contains(e.target)||a.contains(e.target)||(r.startsWith(".")?a.classList.remove(r.substring(1)):r.startsWith("#")&&a.id===r.substring(1)&&(a.id=""))})):console.error("Element not found. Make sure the class/id provided is correct.")},loadDisqusCommentCount=()=>{var e=document.createElement("script");e.src=`https://${disqus_shortname}.disqus.com/count.js`,e.setAttribute("id","dsq-count-scr"),e.async=!0,document.body.appendChild(e)},get_comment=(e,t)=>{var r,s;"disqus"===e?page__url&&page__id&&disqus_shortname?"click"===t?(r=document.querySelector("#comments"))&&(r.innerHTML="",(s=document.createElement("div")).className="bt_disqus",s.id="disqus_thread",s.innerText="Show Comment",r.appendChild(s),r.classList.remove("none"),s.addEventListener("click",e=>{var t;e&&e.target&&!e.target.classList.contains("loaded")&&(e.target.innerText="Loading...",(t=document.createElement("script")).src=`//${disqus_shortname}.disqus.com/embed.js`,document.body.appendChild(t),e.target.classList.add("loaded"))}),document.addEventListener("DOMContentLoaded",loadDisqusCommentCount)):"viewport"===t&&(r=document.querySelector("#comments"))&&(r.innerHTML="",(s=document.createElement("div")).className="bt_disqus loaded",s.id="disqus_thread",s.innerText="Loading...",r.appendChild(s),r.classList.remove("none"),t=new IntersectionObserver((e,r)=>{e.forEach(e=>{var t;e.isIntersecting&&(document.getElementById("disqus_thread")&&(void 0===window.DISQUS?((t=document.createElement("script")).src=`//${disqus_shortname}.disqus.com/embed.js`,t.setAttribute("data-timestamp",+new Date),(document.head||document.body).appendChild(t)):window.DISQUS.reset({reload:!0,config:function(){this.page.identifier=page__id,this.page.url=page__url}})),r.unobserve(e.target))})},{root:null,rootMargin:"0px",threshold:.1}),(s=document.getElementById("disqus_thread"))&&t.observe(s),document.addEventListener("DOMContentLoaded",loadDisqusCommentCount)):console.error("Error: page__url, page__id, or disqus_shortname not available."):"blogger"===e&&(r=document.querySelector("#comments"))&&r.classList.remove("none")},get_cloneElement=(e,t,r)=>{var e=document.querySelector(e),s=document.querySelector(t);s?s.innerHTML=e?e.innerHTML:r:console.warn(`Elemen tujuan '${t}' tidak ditemukan.`)};
```

## Fitur yang tersedia

| Function | Versi |
| ------ | ------ |
| TimeAgo | `1.0.0` `1.0.1` `1.0.2` `1.0.3` `1.0.4` `1.0.5` `1.0.6` `1.0.7` `1.0.8` `1.0.9` `1.1.0` `1.1.1` |
| Move Element | `1.0.0` `1.0.1` `1.0.2` `1.0.3` `1.0.4` `1.0.5` `1.0.6` `1.0.7` `1.0.8` `1.0.9` `1.1.0` `1.1.1` |
| FeedScript | `1.0.0` `1.0.1` `1.0.2` `1.0.3` `1.0.4` `1.0.5` `1.0.6` `1.0.7` `1.0.8` `1.0.9` `1.1.0` `1.1.1` |
| Klik Random Post | `1.0.0` `1.0.1` `1.0.2` `1.0.3` `1.0.4` `1.0.5` `1.0.6` `1.0.7` `1.0.8` `1.0.9` `1.1.0` `1.1.1` |
| Up Button | `1.0.0` `1.0.1` `1.0.2` `1.0.3` `1.0.4` `1.0.5` `1.0.6` `1.0.7` `1.0.8` `1.0.9` `1.1.0` `1.1.1` |
| Show Year | `1.0.0` `1.0.1` `1.0.2` `1.0.3` `1.0.4` `1.0.5` `1.0.6` `1.0.7` `1.0.8` `1.0.9` `1.1.0` `1.1.1` |
| Tabs | `1.0.0` `1.0.1` `1.0.2` `1.0.3` `1.0.4` `1.0.5` `1.0.6` `1.0.7` `1.0.8` `1.0.9` `1.1.0` `1.1.1` |
| Klik function | `1.0.0` `1.0.1` `1.0.2` `1.0.3` `1.0.4` `1.0.5` `1.0.6` `1.0.7` `1.0.8` `1.0.9` `1.1.0` `1.1.1` |
| Custom Komen Disqus dan Blogger | `1.0.0` `1.0.1` `1.0.2` `1.0.3` `1.0.4` `1.0.5` `1.0.6` `1.0.7` `1.0.8` `1.0.9` `1.1.0` `1.1.1` |
| Clone Element | `1.0.9` `1.1.0` `1.1.1` |

## Cara Penggunaan

### TimeAgo (Waktu yang lalu)
Mengubah format tanggal standar menjadi format "X time ago" (Contoh: 2 days ago, 1 year ago).

### Tag HTML

```html
<div class="time" datetime="2023-11-11T12:01:14+08:00">2023-11-11T12:01:14+08:00</div>
```

### Sintak JS default :

```javascript
get_updateTimes();
```
### Sintak JS Untuk custom :

```javascript
get_updateTimes({
  target: "{Nama class/Id}", // Menggunakan Class/Id custom
  ago: false, // true ingin ada teks "ago" dibelakang dan false tidak ingin ada teks "ago" dibelakang
  format: {
    now: "Baru saja",
    minute: "Menit yang lalu",
    hour: "Jam yang lalu",
    day: "Hari yang lalu",
    month: "Bulan yang lalu",
    year: "Tahun yang lalu"
  }
});
```

### Contoh :
```html
<div class="time_ago" datetime="2023-11-11T12:01:14+08:00"></div>

<script>//<![CDATA[
  get_updateTimes({
    target: ".time_ago", // Menggunakan Class/Id custom
    ago: false, // true ingin ada teks "ago" dibelakang dan false tidak ingin ada teks "ago" dibelakang
    format: {
      now: "Baru saja",
      minute: "Menit yang lalu",
      hour: "Jam yang lalu",
      day: "Hari yang lalu",
      month: "Bulan yang lalu",
      year: "Tahun yang lalu"
    }
  });
//]]></script>
```

### Move Element

#### Fungsi get_moveElement
Fungsi ini memindahkan sebuah elemen HTML dari satu tempat ke tempat lain, atau menggantinya dengan teks atau tag HTML jika elemen yang akan dipindahkan tidak ditemukan.

```javascript
get_moveElement('Data_Elemen', 'Tujuan_Elemen', 'Text/Tag HTML');
```

##### Pejelasan
+ `Data_Elemen` : Selector dari elemen yang akan dipindahkan (misalnya, #data untuk elemen dengan id="data").
+ `Tujuan_Elemen` : Selector dari elemen tujuan (misalnya, #tujuan untuk elemen dengan id="tujuan").
+ `Text/Tag HTML` : Teks atau HTML yang akan ditempatkan di elemen tujuan jika elemen yang akan dipindahkan tidak ditemukan.

#### Fungsi get_moveElement2
Fungsi ini memindahkan sebuah elemen HTML dari satu tempat ke tempat lain, atau menghapus elemen tujuan jika elemen yang akan dipindahkan tidak ditemukan.

```javascript
get_moveElement2('Data_Elemen', 'Tujuan_Elemen');
```
##### Pejelasan
+ `Data_Elemen` : Selector dari elemen yang akan dipindahkan (misalnya, #data untuk elemen dengan id="data").
+ `Tujuan_Elemen` : Selector dari elemen tujuan (misalnya, #tujuan untuk elemen dengan id="tujuan").

### BloggerScript V.1.2.2 Custom

### Klik Random Post
Fungsi ini menambahkan event listener ke elemen HTML yang dipilih untuk menampilkan postingan acak saat diklik.

```javascript
get_Random('ID/Class', 'Nama_Label');
```

```javascript
get_Random('ID/Class');
```

##### Pejelasan
+ `ID/Class` : Selector dari elemen HTML yang akan diklik (misalnya, #myButton untuk elemen dengan id="myButton").
+ `Nama_Label` : Label untuk memfilter postingan (opsional).

### Up Button
Fungsi ini menambahkan fungsionalitas ke elemen HTML yang berfungsi sebagai tombol "ke atas". Ketika tombol diklik, halaman akan menggulir ke atas secara halus. Selain itu, tombol dapat diberi kelas CSS tambahan saat pengguna menggulir ke bawah halaman.

```javascript
get_up_button('ID/Class', 'Nama_Class');
```

```javascript
get_up_button('ID/Class');
```

##### Pejelasan
+ `ID/Class` : Selector dari elemen HTML yang akan berfungsi sebagai tombol "ke atas" (misalnya, #upButton untuk elemen dengan id="upButton").
+ `Nama_Class` : Nama kelas CSS yang akan ditambahkan atau dihapus berdasarkan posisi scroll (opsional, misalnya active).

### Show Year
Fungsi ini mengambil tahun saat ini dan menampilkannya di elemen HTML yang dipilih berdasarkan selector yang diberikan.

```javascript
get_year('ID/Class');
```

##### Pejelasan
+ `ID/Class` : Selector dari elemen HTML di mana tahun saat ini akan ditampilkan (misalnya, #year untuk elemen dengan id="year" atau .year untuk elemen dengan class="year".

### Tabs
Fungsi ini mengatur fungsionalitas tab navigasi di halaman web. Ketika sebuah tab diklik, tab tersebut menjadi aktif dan menampilkan konten yang sesuai, sementara tab dan konten lainnya disembunyikan.

```html
<div class="box_tabs">
  <nav data-tabs=".box_tabs .content">
    <span class="tab_cp__ active" data-name="tab_1">Tab 1</span>
    <span class="tab_cp__" data-name="tab_2">Tab 2</span>
  </nav>
  <div class="content active" data-name="tab_1" style="display: block;">Tab 1</div>
  <div class="content" data-name="tab_2" style="display: none;">Tab 2</div>
</div>
```

```javascript
get_tabs();
```

### Klik function
Fungsi ini digunakan untuk menambahkan atau menghapus kelas atau ID pada elemen target saat tombol diklik. Fungsi ini juga menghapus kelas atau ID dari elemen lain yang ditentukan agar hanya satu elemen yang memiliki kelas atau ID tersebut pada satu waktu.

Fungsi `get_klik1` digunakan untuk menambahkan atau menghapus kelas atau ID pada elemen target ketika elemen pemicu diklik.
```javascript
get_klik1('add', 'class yang ditambah', 'class/id button klik', 'class/id target');
get_klik1('remove', 'class yang ditambah', 'class/id button klik', 'class/id target');
```

Fungsi `get_klik2` mirip dengan get_klik1, tetapi dengan tambahan fungsionalitas untuk menghapus kelas atau ID saat mengklik di luar elemen yang ditentukan.
```javascript
get_klik2('add', 'class yang ditambah', 'class/id button klik', 'class/id target');
get_klik2('remove', 'class yang ditambah', 'class/id button klik', 'class/id target');
```

### Custom Komen Disqus dan Blogger
```javascript
get_comment('disqus', 'click'); // Untuk memuat komentar Disqus dengan klik
```

```javascript
get_comment('disqus', 'viewport'); // Untuk memuat komentar Disqus ketika elemen masuk viewport
```

```javascript
get_comment('blogger'); // Untuk memuat komentar blogger
```
