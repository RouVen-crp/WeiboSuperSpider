# WeiboDeepAnalyzer - ����΢����ȷ�������

## ? ���ܽ���

WeiboDeepAnalyzer ��һ��������ȷ�������΢���� Python ���ߣ�����������΢���������ĺ��Ĺ��ܡ�

### ���Ĺ���

1. **΢��������ȡ**

   - ����΢���ı���֧�ֳ�΢��չ����
   - ͼƬ������ȡ
   - ����ʱ�����Դ
   - �����������ݣ����ޡ�ת������������

2. **���������ȡ**

   - ������������
   - ��������Ϣ
   - ����ʱ��͵�����
   - ֧�ַ�ҳ��ȡ

3. **ת����Ϣ�ռ�**

   - ת������Ϣ
   - ת��ʱ����������
   - ת��ʱ��͵�����

4. **����ͳ�Ʒ���**

   - ���廥������ͳ��
   - Top ������/ת��������
   - ƽ�����ݳ��ȷ���

5. **���ʽ���ݵ���**
   - JSON ��ʽ���������ݣ�
   - CSV ��ʽ���������ݱ�
   - �Զ����ļ��д洢

---

## ? ���ٿ�ʼ

### 1. ����׼��

ȷ���Ѱ�װ Python 3.6+����������

```bash
pip install requests lxml pillow
```

### 2. ���� Cookie

**���� 1������ .env �ļ�**

��`�� GUI ���ܶ�����/`Ŀ¼�´���`.env`�ļ���

```
COOKIE=���΢��Cookie�ַ���
```

**���� 2����������**

```bash
# Windows PowerShell
$env:COOKIE="���Cookie�ַ���"

# Linux/Mac
export COOKIE="���Cookie�ַ���"
```

**��ȡ Cookie �Ĳ��裺**

1. ����������� https://weibo.cn
2. ��¼���΢���˺�
3. �� F12 �򿪿����߹���
4. �л��� Network�����磩��ǩ
5. ˢ��ҳ��
6. ������������ҵ� Request Headers �е� Cookie
7. �������� Cookie �ַ���

Cookie ʾ����ʽ��

```
_T_WM=xxxxx; SUBP=xxxxx; SCF=xxxxx; SUB=xxxxx; SSOLoginState=xxxxx
```

### 3. ����ʾ��

```python
from WeiboDeepAnalyzer import WeiboDeepAnalyzer

# �����÷�����������
analyzer = WeiboDeepAnalyzer(wid='QbelLys5Z')
analyzer.analyze()

# ������ȡҳ������ʡʱ�䣩
analyzer = WeiboDeepAnalyzer(wid='QbelLys5Z')
analyzer.analyze(max_comment_pages=10, max_repost_pages=10)
```

---

## ? ��ϸʹ��˵��

### ��ʼ������

```python
WeiboDeepAnalyzer(
    wid='΢��ID',              # ���裺΢��ID��mid
    cookie=None,              # ��ѡ��Cookie�ַ�����Ĭ�ϴӻ�����ȡ��
    output_dir='weibo_analysis'  # ��ѡ�����Ŀ¼��
)
```

### ������������

```python
analyzer = WeiboDeepAnalyzer(wid='QbelLys5Z')

# ��ʽ1��һ����������
analyzer.analyze(
    max_comment_pages=None,  # None��ʾ��ȡ��������
    max_repost_pages=None    # None��ʾ��ȡ����ת��
)

# ��ʽ2���ֲ�ִ��
analyzer.get_weibo_content()           # ����1����ȡ΢������
analyzer.get_all_comments(max_pages=5) # ����2����ȡ���ۣ���5ҳ��
analyzer.get_all_reposts(max_pages=5)  # ����3����ȡת������5ҳ��
analyzer.generate_stats()              # ����4������ͳ��
analyzer.export_json()                 # ����5������JSON
analyzer.export_csv()                  # ����6������CSV
```

### ��ȡ΢�� ID��wid��

#### ���� 1����΢��������ȡ

- **�ֻ�������**��`https://m.weibo.cn/status/QbelLys5Z`

  - wid = `QbelLys5Z`

- **PC ������**��`https://weibo.com/1234567890/QbelLys5Z`

  - wid = `QbelLys5Z`

- **����ҳ����**��`https://weibo.cn/comment/QbelLys5Z`
  - wid = `QbelLys5Z`

#### ���� 2�������� ID ת��

�����������ʽ��΢�� ID������ֱ��ʹ�ã�

```python
analyzer = WeiboDeepAnalyzer(wid='4467107636950632')
```

---

## ? ����ļ�˵��

������ɺ󣬻���`weibo_analysis/{wid}/`Ŀ¼�����������ļ���

### 1. {wid}\_complete.json

������ JSON ��ʽ���ݣ�������

```json
{
  "weibo_content": {
    "wid": "΢��ID",
    "content": "΢������",
    "images": ["ͼƬ����1", "ͼƬ����2"],
    "like_count": ������,
    "repost_count": ת����,
    "comment_count": ������,
    ...
  },
  "comments": [
    {
      "comment_id": "����ID",
      "commenter_name": "�������ǳ�",
      "content": "��������",
      ...
    }
  ],
  "reposts": [...],
  "stats": {
    "interaction_stats": {...},
    "comment_stats": {...},
    "repost_stats": {...}
  }
}
```

### 2. {wid}\_weibo.csv

΢��������Ϣ���
| ΢�� ID | ���� | ���� | ͼƬ�� | ����ʱ�� | ��Դ | ������ | ת���� | ������ | ���� |
|--------|------|------|--------|----------|------|--------|--------|--------|------|

### 3. {wid}\_comments.csv

����������
| ���� ID | ������ ID | �������ǳ� | �������� | ������ | ����ʱ�� |
|--------|----------|------------|----------|--------|----------|

### 4. {wid}\_reposts.csv

ת��������
| ת���� ID | ת�����ǳ� | ת������ | ������ | ����ʱ�� |
|----------|------------|----------|--------|----------|

### 5. {wid}\_stats.csv

ͳ�Ʒ������
| ͳ���� | ��ֵ |
|--------|------|
| �ܵ����� | xxx |
| ��ת���� | xxx |
| �������� | xxx |

---

## ? ʹ�ó���

### ���� 1���������

```python
# ����ĳ������΢�������������ʹ���·��
analyzer = WeiboDeepAnalyzer(wid='����΢��ID')
analyzer.analyze(max_comment_pages=50)  # ��������ҳ����ʡʱ��

# �鿴Top������
print(analyzer.stats['comment_stats']['top_commenters'])
```

### ���� 2��Ӫ��Ч������

```python
# ����Ʒ��΢���Ļ���Ч��
analyzer = WeiboDeepAnalyzer(wid='Ʒ��΢��ID')
analyzer.analyze()

# ��ȡת���û��б����ں�������
reposts_df = pd.read_csv(f'weibo_analysis/{wid}/{wid}_reposts.csv')
```

### ���� 3��ѧ���о�

```python
# ������������΢��
wids = ['wid1', 'wid2', 'wid3']
for wid in wids:
    analyzer = WeiboDeepAnalyzer(wid=wid)
    analyzer.analyze(max_comment_pages=20, max_repost_pages=20)
    time.sleep(10)  # �����������
```

### ���� 4����Ʒ����

```python
# ֻ��ȡ�������ݽ����ı�����
analyzer = WeiboDeepAnalyzer(wid='��Ʒ΢��ID')
analyzer.get_weibo_content()
comments = analyzer.get_all_comments(max_pages=30)

# �Զ��崦����������
for comment in comments:
    print(f"{comment['commenter_name']}: {comment['content']}")
```

---

## ?? �߼�����

### �Զ���������ʱ

�޸�Դ���е���ʱ��������Ӧ��ͬ���绷����

```python
# ��get_all_comments������
time.sleep(random.uniform(1.5, 3))  # Ĭ��1.5-3��

# ��get_all_reposts������
time.sleep(random.uniform(1.5, 3))  # Ĭ��1.5-3��
```

### �������Դ���

```python
# ��_request������
def _request(self, url, timeout=10, retry=3):  # retry�����������Դ���
```

### �Զ������Ŀ¼

```python
analyzer = WeiboDeepAnalyzer(
    wid='QbelLys5Z',
    output_dir='custom_output'  # �Զ������Ŀ¼
)
```

---

## ?? �����ܹ�

### �����ع�˵��

���������������������������ĺ��Ĺ��ܣ�

1. **WeiboUserScrapy.py**

   - ���ã�HTML ������ʱ�������������ȡ�߼�
   - ���죺�������û�΢����ȡ��Ϊ����΢�������ȡ

2. **WeiboRepostSpider.py**

   - ���ã�ת��ҳ������߼�
   - ���죺���ϵ�ͳһ�ķ���������

3. **WeiboCommentScrapy.py**

   - ���ã�����ҳ��������û���Ϣ��ȡ
   - ���죺�����̣�ȥ�����̣߳���Ϊ˳��ִ��

4. **WeiboSuperCommentScrapy.py**
   - �ο������� ID ת���߼���mid2id��
   - ˵����δʹ�õ�¼��ʽ������ʹ�� Cookie �ļ򵥷�ʽ

### ����ģ��

```
WeiboDeepAnalyzer
������ __init__()          # ��ʼ������
������ _request()          # HTTP�����װ
������ _parse_html()       # HTML����
������ _parse_time()       # ʱ���ʽת��
������ get_weibo_content() # ΢��������ȡ
������ get_all_comments()  # ������ȡ
������ get_all_reposts()   # ת����ȡ
������ generate_stats()    # ͳ�Ʒ���
������ export_json()       # JSON����
������ export_csv()        # CSV����
������ analyze()           # ������������
```

---

## ?? ע������

### Cookie ���

1. **Cookie ��Ч��**��Cookie ����ڣ���������¼��ʾ�����»�ȡ
2. **�˺Ű�ȫ**����Ҫ�� Cookie ���������
3. **���˺�**������ʹ��С�Ž������ݲɼ�

### ��ȡ����

1. **Ƶ������**�����������ܱ����Ʒ��ʣ����飺

   - ���η������ 10 ������
   - ��Ҫͬʱ���ж��ʵ��
   - ҹ��ʱ����ȡ�ɹ��ʸ���

2. **����������**��

   - ������΢����10 ��+���ۣ���������ҳ��
   - ʹ��`max_comment_pages`��`max_repost_pages`����

3. **��������**��
   - ���������������ʱ
   - �ѹر�ϵͳ��������ͻ
   - ����ر� VPN

### ������

�������󼰽��������

1. **"COOKIE Ϊ��"**

   - ���.env �ļ��Ƿ���ȷ����
   - ��� Cookie ��ʽ�Ƿ���ȷ

2. **"�޷�����΢��ҳ��"**

   - Cookie �ѹ��ڣ������»�ȡ
   - ������������
   - ΢�� ID �����ڻ���ɾ��

3. **"HTTP 302/403"**

   - ������챻���ƣ��ȴ�������
   - Cookie ʧЧ

4. **����ʧ��**
   - ΢��ҳ��ṹ�仯
   - �����ʽ΢������Ƶ�����µȣ�

---

## ? ���ܲο�

����ʵ�����ݣ����绷����100Mbps �������

| ������     | ����ҳ�� | ת��ҳ�� | Ԥ�ƺ�ʱ   | �����С |
| ---------- | -------- | -------- | ---------- | -------- |
| С��΢��   | 10       | 10       | 1-2 ����   | <1MB     |
| ����΢��   | 50       | 50       | 5-10 ����  | 5-10MB   |
| ����΢��   | 100+     | 100+     | 20-30 ���� | 20-50MB  |
| ������΢�� | 500+     | 500+     | 1-2 Сʱ   | 100MB+   |

**�Ż�����**��

- ���ڳ�����΢���������������ȡ
- ʹ��`max_pages`����������ȡ��Χ
- ������ȡ����Ҫ�����ݣ������ۣ�

---

## ? ��ԭ�д���Ա�

### ԭ�д����ص�

- **WeiboUserScrapy.py**��������ȡ�û�����΢��
- **WeiboCommentScrapy.py**��������ȡ����
- **WeiboRepostSpider.py**��������ȡת��
- **��ɢ**�����ܷ�ɢ�ڶ���ļ�

### WeiboDeepAnalyzer ����

? **��������**��һ����������й���  
? **�������**��רע�ڵ���΢������������  
? **�ṹ�����**��ͳһ�� JSON+CSV ��ʽ  
? **ͳ�Ʒ���**�����û������ݷ���  
? **����ʹ��**��һ�д��������������  
? **����չ**��ģ�黯��Ʊ��ڶ��ο���

---

## ? �����뷴��

### ���ⷴ��

���������⣬���ṩ��

1. ����������Ϣ
2. ʹ�õ�΢�� ID
3. Python �汾�������汾

### ���ܽ���

��ӭ����¹����������磺

- ͼƬ���ع���
- ��Ƶ������ȡ
- ����ͳ��ά��
- ���ݿ��ӻ�

---

## ? ���˵��

�����߽���ѧϰ���о�ʹ�ã�����������ԭ��

1. **�Ϸ�ʹ��**������ȡ�������ݣ����ַ��û���˽
2. **����Ƶ��**������΢����������ɹ��󸺵�
3. **���ݱ���**�����Ʊ�����ȡ������
4. **��ҵʹ��**��������ҵʹ��������΢���û�Э��

---

## ? ��ϵ��ʽ

- ����ԭ���ߴ����ع���inspurer(��Сˮ��)
- ԭ���� GitHub: https://github.com/inspurer
- ΢�Ź��ں�: ��Сˮ��(ID: inspurer)

---

## ? ������־

### v1.0.0 (2025-10-29)

- ? ��ʼ�汾����
- ? �������д��빦��
- ? ʵ�ֵ���΢����ȷ���
- ? ֧�� JSON �� CSV ˫��ʽ����
- ? ����ͳ�Ʒ�������
