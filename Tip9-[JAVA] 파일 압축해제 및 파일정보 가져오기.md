---

---

# 파일에 관하여...

프로젝트를 진행하면서 파일을 어떻게 읽고 쓰며, 파일 객체 그리고 stream을 이용하여 파일데이터를 분석할 수 있는 기회를 갖게 되었다.

이전에는, 단지 파일을 multiparthttpservlerRequest객체를 이용하여 데이터를 받고 새로운 파일을 생성하고 해당 경로와 실제 파일명을 db에 넣어주어 파일을 컨트롤하는 방법만 알고 있었다.  

그러나, 이번에는 실제 파일데이터를 가져와서 해쉬값을 가져오기도 하고, file header의 값을 가져오는 라이브러리를 사용하여 파일구조와 데이터의 형태를 가져옴으로써 데이터에 대한 지식이 깊어진 거 같다.

먼저, 코드를 보자 해당코드는. zip파일 일 경우 파일을 압축해제하여 각 각의 파일에 대한 정보, 그리고 해쉬값, 헤더값을 통해 디지털서명여부를 가져오고 exe파일일 경우 해당 파일에 대한 정보, 해쉬값, 헤더값의 디지털 서명여부를 가져오는 코드이다.

여기서 PE class는 pecoff4j.jar라는 라이브러리에서 가져오는데 pe파일 헤더정보를 얻기위해 사용되었다.

*PE파일이란 protable Excutable 의 약자로 윈도우 상에서 실행할 수 있는 프로그램을 뜻한다.                                         => ex) exe, scr, sys, vxd, dll, cpl, drv 등등....

```
@Repository
public class FileDAO {

	public JSONArray getFilesList(MultipartHttpServletRequest mreq) throws Exception {
		//date형식 지정
		String pattern = "yyyy-MM-dd HH:mm:ss";
		//date형식 변환저장
		SimpleDateFormat sdf = new SimpleDateFormat(pattern);
		//결과값 
		JSONArray jsonArr = new JSONArray();
		//현재시간
		long time = System.currentTimeMillis();
		//프로젝트 루트경로=webapp/upload
		String path = mreq.getSession().getServletContext().getRealPath("/upload");
		//파일데이터
		MultipartFile mf = mreq.getFile("uploadfile");
		//파일데이터 빈값 체크
		if (!mf.isEmpty()) {
			//파일명
			String orgName = mf.getOriginalFilename();
			//파일확장자 체크
			int pos = orgName.lastIndexOf(".");
			//파일명
			String orgName2 = orgName.substring(0, pos);
			//파일확장자명
			String ext = orgName.substring(pos + 1);
			//중복없는 새이름
			String newName = orgName2 + time + mf.getSize() + "." + ext;
			//해쉬알고리즘 정의
			MessageDigest md5 = MessageDigest.getInstance("MD5");
			//해쉬적용 stream 객체
			DigestInputStream di;
			//확장자 체크
			if (ext.equals("zip")) {
				//빈파일 생성
				File file = new File(path + File.separator + newName);
				//빈파일에 파일데이터 전송
				mf.transferTo(file);
				//zip파일객체로 받을 시 여러가지 사용가능 but 여기서는 쓰지않는다.
//				ZipFile zipfile = new ZipFile(path + "/" + newName);
//				System.out.println(zipfile.getName());
//				System.out.println(zipfile.size());
				//파일 stream형태로 데이터 수집
				FileInputStream fis = new FileInputStream(file);
				//zip stream형태로 변환
				ZipInputStream zis = new ZipInputStream(fis, Charset.forName("EUC-KR"));
				//각 파일 entry체크를 위한 클래스
				ZipEntry zipentry = null;
				//파일 entry 루프돌면서 체크
				while ((zipentry = zis.getNextEntry()) != null) {
					pos = zipentry.getName().lastIndexOf(".");
					ext = zipentry.getName().substring(pos + 1);
					if (ext.equals("exe")) {
						JSONObject jsonObj = new JSONObject();
						//파일생성
						File tfile = new File(path + File.separator + zipentry.getName());
						//파일데이터 write준비
						FileOutputStream fos = new FileOutputStream(tfile);
						int len;
						//한번에 쓸파일 크기 지정 stream이므로 byte타입으로
						byte buffer[] = new byte[1024];
						//read -> return len = 저장하고 읽은 바이트 수(사이즈)를 저장
						//read(buffer) = buffer에 저장하고 읽은 바이트 저장
						while ((len = zis.read(buffer)) > 0) {
							//buffer의 0위치부터 len size만큼의 바이트 쓰기
							fos.write(buffer, 0, len);
						}
						//버퍼에 있는 데이터를 내보내고 버퍼를 비워버림
						//write시 사용하지 않을 경우 간혹 데이터가 실제로 안들어가는 경우가 생김.
						fos.flush();
						fos.close();
						//데이터가 생긴 파일을 읽어드림
						FileInputStream tfis = new FileInputStream(tfile);
						//버퍼를 통해서 읽기 = 완충역할
						BufferedInputStream bi = new BufferedInputStream(tfis);
						//해쉬변환을 위한 타입변환 md5에 읽은준비
						di = new DigestInputStream(bi, md5);
						//digestInputStream의 데이터 모두 읽기
						while (di.read() != -1)
							;
						//해쉬값(다이제스트) 얻기
						byte[] hash = md5.digest();
						// 데이터정보 저장
						jsonObj.put("fileName", zipentry.getName());
						jsonObj.put("size", Long.toString(zipentry.getSize()));
						jsonObj.put("hash", byteArray2Hex(hash));
						jsonObj.put("create_date", sdf.format(new Date(zipentry.getLastModifiedTime().toMillis())));
						jsonArr.add(jsonObj);
						System.out.println(jsonObj);

						//md5로 데이터를 모두 움직였기 때문에 파일 다시 읽기
						FileInputStream pefis = new FileInputStream(tfile);
						//버퍼를 통해서 읽기 = 완충역할
						BufferedInputStream pebi = new BufferedInputStream(pefis);
						//PE클래스를 통해서 데이터 파싱
						PE pe = PEParser.parse(pebi); // 스트림형태
						//헤더값 존재여부 체크
						if (pe.getOptionalHeader() != null) { // 헤더여부 확인
							//optionheader값 가져오기
							OptionalHeader oh = pe.getOptionalHeader();
							//특정위치 데이터(이미지데이터디렉토리) 가져오기
							ImageDataDirectory[] idarr = oh.getDataDirectories();
//							for (int j = 0; j < idarr.length; j++) {
//								System.out.println(j);
//								System.out.println(idarr[j].toString());
//								System.out.println(idarr[j].hashCode());
//								System.out.println(idarr[j].getVirtualAddress());
//								System.out.println(idarr[j].getSize());
//							}
							// 서명여부 확인 4번 index에 값 체크를 통해
							if (idarr[4].getSize() != 0) {
								jsonObj.put("sign_yn", "y");
							} else {
								jsonObj.put("sign_yn", "n");
							}
						} else {
							jsonObj.put("sign_yn", "n");
						}
						pebi.close();
						di.close();
						bi.close();
						tfis.close();
						//압축해제파일 삭제. *삭제전 stream닫아주기!
						boolean result = tfile.delete();
					}
				}
				zis.close();
				fis.close();
			} else if (ext.equals("exe")) {
				JSONObject jsonObj = new JSONObject();
				File file = new File(path + File.separator + newName);
				mf.transferTo(file);
				// zip파일 체크
				FileInputStream fis = new FileInputStream(file);
				BufferedInputStream bi = new BufferedInputStream(fis);
				di = new DigestInputStream(bi, md5);
				while (di.read() != -1)
					;
				byte[] hash = md5.digest();
				jsonObj.put("fileName", orgName);
				jsonObj.put("size", Long.toString(file.length()));
				jsonObj.put("hash", byteArray2Hex(hash));
				jsonObj.put("create_date", sdf.format(new Date(file.lastModified())));
				jsonArr.add(jsonObj);
				di.close();
				bi.close();
				fis.close();
			} else {
				JSONObject jsonObj = new JSONObject();
				jsonObj.put("disable", "exe 또는 zip파일 이외의 확장자는 업로드 할 수 없습니다.");
				jsonArr.add(jsonObj);
			}
			System.out.println(jsonArr);
		}
		return jsonArr;
	}
	//바이트타입 hax타입변환
	private static String byteArray2Hex(byte[] hash) {
		Formatter formatter = new Formatter();
		for (byte b : hash) {
			formatter.format("%02x", b);
		}
		return formatter.toString();
	}
}
```

| InputStream에 정의된 메소드                            | 설명                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| int available() throws IOException                     | - 현재 읽을 수 있는 바이트 수를 얻는다.                      |
| void close() throws IOException                        | - InputStream을 닫는다                                       |
| void mark(int readlimit)                               | - InputStream에서 현재 위치를 표시한다.                      |
| boolean markSupported()                                | - 해당 InputStream에서 mark()로 지정된 지점이 있는지 여부를 체크한다. |
| abstract int read() throws IOException                 | - InputStream에서 한 바이트를 읽어서 int 값으로 반환한다.    |
| int read(byte[] b) throws IOException                  | - byte[] b 만큼의 데이터를 읽어서 b에 저장하고읽은 바이트 수를 반환한다 |
| int read(byte[] b, int off, int len)thrwos IOException | - len 만큼을 읽어 byte[] b의 off위치에 저장하고 읽은 바이트 수를 반환한다 |
| void reset()                                           | - mark() 를 마지막으로 호출한 위치로 이동한다.               |
| long skip(long n) throws IOException                   | - InputStream에서 n 바이트 만큼 데이터를 스킴하고 바이트 수를 반환한다. |

| OutputStream에 정의된 메소드                                 | 설명                                                 |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| void close() throws IOException                              | - OutputStream을 닫는다                              |
| void flush() throws IOException                              | - 버퍼에 남은 출력 스트림을 출력한다.                |
| abstract void write(int i) throws IOException                | - 정수 i의 하위 8비트를 출력한다.                    |
| void write(byte buf[]) throws IOException                    | - buf의 내용을 출력한다.                             |
| void write(byte buf[], int index, int size) throws IOException | - buf의 index 위치부터 size만큼의 바이트를 출력한다. |

![](.\img\img01.jpg)



stream에서 꼭 이해해야할 개념이 하나 있다. stream에 담기는 데이터는 read나 write를 할 경우 stream내부에 데이터는 복사되는 것이 아닌 이동되는 것이므로! 사용하고나면 데이터가 없다. 즉, 사용후에는 close를 하면 되겠고 사용하고 같은 데이터를 가지고 다시 무언가가 하고 싶을 경우에는 해당 파일로부터 다시 stream으로 데이터를 읽어서 처리해야한다.

필자는 재사용이 가능하다고 생각을 갖고 있어서, 위에 작업을 하는데 꽤나 시간이 걸렸다 ㅜㅜ.

 데이터의 이동! 꼭 기억하자. 