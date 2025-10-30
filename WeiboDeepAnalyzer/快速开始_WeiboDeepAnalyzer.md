# WeiboDeepAnalyzer ���ٿ�ʼָ��

## ? 5 ��������

### ��һ������װ����

```bash
pip install requests lxml pillow
```

### �ڶ��������� Cookie

��`�� GUI ���ܶ�����/`Ŀ¼�´���`.env`�ļ���

```
COOKIE=���΢��Cookie�ַ���
```

**��λ�ȡ Cookie��**

1. ��������� https://weibo.cn ����¼
2. �� F12 �򿪿����߹��� �� Network ��ǩ
3. ˢ��ҳ�� �� �����������
4. ���� Request Headers �е� Cookie ֵ

### ������������ʾ��

**���� 1��ʹ�ò��Խű����Ƽ���**

```bash
cd "�� GUI ���ܶ�����"
python test_deep_analyzer.py
```

ѡ��"1"���п��ٲ��ԣ���ѡ��"4"�����Զ�����ԡ�

**���� 2����д�Լ��Ĵ���**

```python
from WeiboDeepAnalyzer import WeiboDeepAnalyzer

# �����������������ۺ�ת����10ҳ��
analyzer = WeiboDeepAnalyzer(wid='���΢��ID')
analyzer.analyze(max_comment_pages=10, max_repost_pages=10)

print(f"������ɣ����������: {analyzer.wid_dir}")
```

---

## ? ��ȡ΢�� ID �ķ���

### ���� 1����΢��������ȡ

- �ֻ��ˣ�`https://m.weibo.cn/status/QbelLys5Z` �� ID �� `QbelLys5Z`
- PC �ˣ�`https://weibo.com/1234567890/QbelLys5Z` �� ID �� `QbelLys5Z`

### ���� 2��������ҳ��

����΢�����飬�������ַ���У�

- `https://weibo.cn/comment/QbelLys5Z` �� ID �� `QbelLys5Z`

---

## ? ����ļ�˵��

������ɺ���� `weibo_analysis/{΢��ID}/` ���������ļ���

```
weibo_analysis/
������ QbelLys5Z/
    ������ QbelLys5Z_complete.json    # ����JSON����
    ������ QbelLys5Z_weibo.csv        # ΢��������Ϣ
    ������ QbelLys5Z_comments.csv     # ��������
    ������ QbelLys5Z_reposts.csv      # ת������
    ������ QbelLys5Z_stats.csv        # ͳ������
```

---

## ? �����÷�

### 1. ���ٲ��ԣ�С��������

```python
analyzer = WeiboDeepAnalyzer(wid='΢��ID')
analyzer.analyze(max_comment_pages=5, max_repost_pages=5)
```

### 2. �����������������ݣ�

```python
analyzer = WeiboDeepAnalyzer(wid='΢��ID')
analyzer.analyze()  # ������ҳ������ȡȫ��
```

### 3. ֻ��ȡ����

```python
analyzer = WeiboDeepAnalyzer(wid='΢��ID')
analyzer.get_weibo_content()
analyzer.get_all_comments(max_pages=20)
analyzer.export_json()
```

### 4. ������������΢��

```python
wids = ['wid1', 'wid2', 'wid3']
for wid in wids:
    analyzer = WeiboDeepAnalyzer(wid=wid)
    analyzer.analyze(max_comment_pages=10, max_repost_pages=10)
    time.sleep(10)  # �����������
```

---

## ?? ��Ҫ��ʾ

### 1. Cookie ���

- Cookie ����ڣ�������¼��ʾ�����»�ȡ
- ��Ҫ������� Cookie ������
- ����ʹ��С�Ž������ݲɼ�

### 2. ��ȡ����

- �������ᱻ���ƣ����飺
  - ���η������� 10 ������
  - ҹ��ʱ�γɹ��ʸ���
  - �ر� VPN
- ������΢����������ҳ��

### 3. ��������

**"COOKIE Ϊ��"**

- ���.env �ļ��Ƿ���ȷ����
- ��� Cookie ��ʽ�Ƿ�������

**"�޷�����΢��ҳ��"**

- Cookie �ѹ��ڣ������»�ȡ
- ΢�� ID �����ڻ���ɾ��

**"HTTP 302/403"**

- ������챻���ƣ��ȴ� 10 ���Ӻ�����
- Cookie ʧЧ

---

## ? ���ܲο�

| ���ݹ�ģ | ����ҳ�� | ת��ҳ�� | Ԥ�ƺ�ʱ |
| -------- | -------- | -------- | -------- |
| ����     | 5        | 5        | 1 ����   |
| С��     | 10       | 10       | 2 ����   |
| ����     | 50       | 50       | 10 ����  |
| ����     | 100+     | 100+     | 30 ����+ |

---

## ? ������Ϣ

- ��ϸ�ĵ���[WeiboDeepAnalyzer_README.md](WeiboDeepAnalyzer_README.md)
- ���Խű���[test_deep_analyzer.py](test_deep_analyzer.py)
- ������[WeiboDeepAnalyzer.py](WeiboDeepAnalyzer.py)

---

## ? ��һ��

1. **���ݷ���**��ʹ�� pandas ��ȡ CSV �ļ��������ݷ���

   ```python
   import pandas as pd
   df = pd.read_csv('weibo_analysis/QbelLys5Z/QbelLys5Z_comments.csv')
   print(df.head())
   ```

2. **�ı�����**�����������ݽ�����з������ؼ�����ȡ��

3. **���ӻ�**��ʹ�� matplotlib �� seaborn ���ƻ�������ͼ

4. **��չ����**���������д�������Զ��幦��

---

**��ʼ̽���ɣ�** ?
