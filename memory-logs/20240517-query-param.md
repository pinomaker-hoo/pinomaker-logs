# 2024.05.17(금) Memory Logs

## 내용

NestJS + TypeORM을 이용하여 개발하던 중 Dto Validation에 대해 메모합니다.

<br />
<br />

## 1. @Type

<br />

```
Query Param으로 받는 값들은 Dto에서 number라고 선언해도, string으로 들어오게 된다. Query Param은 String이기 때문이다.
```

<br />
<br />

NestJS를 사용하여 Query Param을 받고 싶다면 아마 아래와 같이 작성할 것이다.

```typescript
  @ApiBearerAuth('access-token')
  @ApiOperation({ summary: '노트 리스트 조회' })
  @ApiResponse(NoteResponse.findNoteList[200])
  @UseGuards(JwtAccessGuard)
  @Get('/')
  public async findNoteList(@Query(ValidationPipe) dto: RequestNoteFindDto) {
    const [data, count] = await this.noteService.findNoteList(dto);

    return CommonResponse.createResponse({
      data: { data, count },
      statusCode: 200,
      message: NoteMessage.SUCCESS_FIND_NOTE_LIST.ko,
    });
  }
```

위에 코드를 사용보면 @Query에 대해 VlidationPipe를 거치기에 자동으로 형변환이 된다고 생각했는 데, String으로 넘어왔다.

이는 위에서 언급한 대로 Query Param은 URL에 담긴 값이기에 숫자로 표기되더라도 String의 타입을 지니고 있기에 형 변환을 해줘야한다.

```typescript
// ** Swagger Imports
import { ApiProperty } from "@nestjs/swagger";

// ** Pipe Imports
import { IsEnum, IsNumber, IsOptional } from "class-validator";
import { Type } from "class-transformer";

// ** Enum Imports
import RequestPagingDto from "@/src/global/dto/paging.dto";
import MemberTypeEnum from "@/src/global/enum/memberType.enum";

export default class RequestNoteFindDto extends RequestPagingDto {
  @ApiProperty({ example: 1, required: false })
  @IsOptional()
  @Type(() => Number) // 형 변환
  @IsNumber()
  clubId: number;

  @ApiProperty({ example: MemberTypeEnum.FR, enum: MemberTypeEnum })
  @IsEnum(MemberTypeEnum)
  type: MemberTypeEnum;
}
```

위의 Dto를 보면 clubId에 대해 class-trasnformer에서 제공해주는 Type 데코레이터를 사용하여 String으로 들어올 것을 Number로 변경하는 것을 알 수 있다.

이렇게 하면 clubId르 number로 받을 수 있다.
